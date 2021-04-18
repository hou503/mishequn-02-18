---
title: Introduction to Redis(2)
date: 2020-11-13 20:27:15
tags: Redis
category: Redis
---
Redis is one of the hottest in-memory databases, and by reading and writing data in memory, it greatly increases the read and write speed, so it can be said that Redis is an indispensable part of achieving high concurrency on a website.

When we use Redis, we are exposed to Redis' five object types (string, hash, list, collection, and ordered collection), and the richness of the types is one of Redis' major advantages over Memcached and others. On top of understanding the usage and characteristics of the 5 object types of Redis, it is helpful to further understand the memory model of Redis, such as.

1. Estimate the memory usage of Redis. To date, the cost of memory usage is still relatively high, so you can't be reckless with the use of memory; according to the needs of a reasonable assessment of Redis memory usage, choose the appropriate machine configuration, you can save costs while meeting the demand.

2. Optimize memory usage. Understanding the Redis memory model can help you choose more appropriate data types and encodings to better utilize Redis memory.

3. Analyze and solve problems. When Redis has problems such as blocking and memory usage, you can find out the cause of the problem as soon as possible to analyze and solve the problem.

This article introduces the Redis memory model (3.0 as an example), including Redis memory usage and how to query it, how different object types are encoded in memory, memory allocators (jemalloc), simple dynamic strings (SDS), RedisObject, etc.; and then introduces several applications of the Redis memory model on this basis.

## Redis memory statistics

After the client connects to the server via redis-cli, you can view the memory usage through the info command.

```
info memory
```
![](1.jpg)

The info command can display a lot of information about the redis server, including basic server information, CPU, memory, persistence, client connection information, and so on; memory is a parameter, which means only memory-related information is displayed.

A few of the more important statements in the returned results are as follows.

(1) used_memory: The total amount of memory (in bytes) allocated by the Redis allocator, including the virtual memory used (i.e. swap); the Redis allocator will be introduced later. used_memory_human just shows more friendly.

(2) used_memory_rss: the amount of memory (in bytes) occupied by the Redis process is the same as that seen by the top and ps commands; in addition to the memory allocated by the allocator, used_memory_rss also includes the memory needed by the process itself, memory fragments, etc., but not virtual memory.

Thus, used_memory and used_memory_rss, the former is the amount obtained from the Redis perspective, and the latter is the amount obtained from the operating system perspective. The difference between the two is due to memory fragmentation and the memory required by Redis processes to run, which makes the former smaller than the latter, and the existence of virtual memory, which makes the former larger than the latter.

As the amount of data in Redis is relatively large in actual applications, the memory occupied by running processes is much smaller than the amount of Redis data and memory fragmentation; therefore, the ratio of used_memory_rss and used_memory becomes a parameter to measure the Redis memory fragmentation rate; this parameter is mem_fragmentation_ratio.

(3) mem_fragmentation_ratio: memory fragmentation ratio, the ratio of used_memory_rss / used_memory.

mem_fragmentation_ratio: the ratio of used_memory_rss / used_memory. mem_fragmentation_ratio is greater than 1, and the larger the value, the larger the memory fragmentation ratio. mem_fragmentation_ratio<1 means that Redis uses virtual memory, because the medium of virtual memory is disk, which is much slower than the memory speed. It should be addressed promptly, such as adding Redis nodes, increasing Redis server memory, optimizing applications, etc.

In general, mem_fragmentation_ratio around 1.03 is a healthy state (for jemalloc); the mem_fragmentation_ratio value in the above screenshot is large because no data has been deposited into Redis yet, and the memory in which the Redis process itself is running makes the used_fragmentation_ratio value large. memory_rss is much larger than used_memory.

(4) mem_allocator: Redis uses a memory allocator, specified at compile time; it can be libc, jemalloc or tcmalloc, the default is jemalloc; the one used in the screenshot is the default jemalloc.

## Redis memory partitioning

Redis, as an in-memory database, stores mainly data (key-value pairs) in memory; as you can see from the previous description, in addition to data, other parts of Redis also occupy memory.

The memory consumption of Redis can be divided into the following main parts.

1. Data
As a database, the data is the main part; the memory occupied by this part is counted in used_memory.

Redis uses key-value pairs to store data, where the values (objects) consist of 5 types, namely strings, hashes, lists, collections, and ordered collections. These five types are provided externally by Redis, and in fact, there may be two or more internal coded implementations of each type within Redis; in addition, Redis does not throw data directly into memory when storing objects, but rather wraps objects in various ways: redisObject, SDS, etc. Later in this article, we will focus on data storage in Redis. The details.

2. The memory required to run the process itself
The main Redis process itself must use memory to run, such as code, constant pools, and so on; this memory is in the order of a few megabytes, which is negligible in most production environments compared to the memory used by Redis data. This memory is not allocated by jemalloc, so it is not counted in used_memory.

Additional note: In addition to the main process, Redis also uses memory for running child processes, such as those created when Redis performs AOF and RDB rewrites. Of course, this memory does not belong to Redis processes and is not counted in used_memory or used_memory_rss.

3. Buffer memory
Buffered memory includes client buffers, copy backlog buffers, and AOF buffers; the client buffer stores the input and output buffers for client connections; the copy backlog buffer is used for partial replication functions; and the AOF buffer is used to save the most recent write commands when performing an AOF rewrite. It is not necessary to know the details of these buffers before knowing the corresponding functions; this part of memory is allocated by jemalloc, so it is counted in used_memory.

4. Memory fragmentation
Memory fragmentation is created by redis during the process of allocating and reclaiming physical memory. For example, if changes to data are frequent and the size of the data varies greatly from one another, it may result in memory fragmentation when the space freed by redis is not freed in physical memory, but redis is unable to use it efficiently. Memory fragmentation is not counted in used_memory.

The generation of memory fragmentation is related to the operations performed on the data, the characteristics of the data, etc. In addition, it is also related to the memory allocator used: if the memory allocator is properly designed, the generation of memory fragmentation can be reduced as much as possible. The jemalloc we'll talk about later is a good way to control memory fragmentation.

If the memory fragmentation in the Redis server is already large, it can be reduced by a safe reboot: after the reboot, Redis re-reads the data from the backup file, reorganizes it in memory, and selects the appropriate memory cell for each data to reduce memory fragmentation.

## Redis data store details
1. Overview
The details of the Redis data store involve memory allocators (such as jemalloc), simple dynamic strings (SDS), the five object types and their internal coding, and redisObject.

The following diagram shows the data model involved in executing set hello world.

![](2.jpg)


(1) dictEntry: Redis is a Key-Value database, so for each key-value pair will have a dictEntry, which stores pointers to Key and Value; next points to the next dictEntry, which has nothing to do with the Key-Value.

(2) Key: Key ("hello") is not directly stored as a string, but is stored in the SDS structure, visible in the upper right corner of the figure.

(3) redisObject: Value("world") is neither stored directly as a string nor in SDS like Key, but in a redisObject. In fact, Value is stored in redisObject regardless of which of the five types it is; the type field in redisObject specifies the type of the Value object, and the ptr field points to the address of the object. However, it can be seen that string objects, although wrapped in redisObject, still need to be stored via SDS.

In fact, in addition to the type and ptr fields, there are other fields in redisObject that are not given in the diagram, such as those used to specify the internal encoding of the object; these are described in detail later.

(4) jemalloc: Whether it is a DictEntry object, a redisObject, or an SDS object, a memory allocator (such as a jemalloc) is required to allocate memory for storage. The DictEntry object, for example, is composed of three pointers, accounting for 24 bytes in a 64-bit machine. jemalloc will allocate 32 bytes of memory for it.

The following is an introduction to jemalloc, redisObject, SDS, object types and internal coding, respectively.

2. jemalloc
Redis will specify a memory allocator at compile time; the memory allocator can be libc, jemalloc or tcmalloc, and the default is jemalloc.

The default is jemalloc. jemalloc is the default memory allocator for Redis, and it does a relatively good job of reducing memory fragmentation. jemalloc divides the memory space in a 64-bit system into three ranges: small, large and huge.

The memory units divided by jemalloc are shown in the following figure.

![](3.jpg)


3. redisObject
As mentioned earlier, there are five types of Redis objects; regardless of the type, Redis does not store them directly, but through the redisObject object.

The redisObject object is very important. Redis object types, internal coding, memory recycling, shared objects, etc., all require redisObject support.

The definition of a redisObject is as follows (may vary slightly from version to version of Redis).

![](4.jpg)

(1) type
The type field represents the type of the object and is four bits long; it currently includes REDIS_STRING (string), REDIS_LIST (list), REDIS_HASH (hash), REDIS_SET (set), and REDIS_ZSET (ordered set).

When we execute the type command, we are reading the type field of RedisObject to get the type of the object; the following image shows.
![](5.jpg)
(2) encoding
encoding represents the internal encoding of an object and takes up 4 bits.

For each type supported by Redis, there are at least two encodings. For example, for strings, there are int, embstr, and raw encodings. Through the encoding property, Redis can set different encodings for objects according to different usage scenarios, which greatly improves the flexibility and efficiency of Redis. Redis uses compressed lists to store the elements in a list, which takes up less memory and can be loaded faster than double-ended lists.

The object encoding command allows you to see how the object is encoded, as shown below.

![](6.jpg)

(3) lru
The lru records the last time an object was accessed by the command program, and the number of bits it occupies varies by version (e.g., 24 bits for version 4.0 and 22 bits for version 2.6).

The object idle time can be calculated by comparing the lru time with the current time; the object idletime command displays the idle time in seconds.


![](7.jpg)

The lru value is not only printed by the object idletime command, but also has to do with Redis memory recuperation: if Redis has the maxmemory option turned on and the memory recuperation algorithm chooses volatile-lru or allkeys-lru, then when Redis memory exceeds the value specified by maxmemory, Redis will prioritize the object with the longest idle time to be freed.

(4) refcount
refcount and shared objects

refcount records the number of times the object has been referenced, type is integer. refcount's role, mainly in the object's reference count and memory recovery. When a new object is created, refcount is initialized to 1; when a new program uses the object, refcount is added to 1; when the object is no longer used by a new program, refcount is subtracted from 1; when refcount becomes 0, the memory occupied by the object will be freed.

The object that has been used many times in Redis (refcount>1) is called a shared object.Redis saves memory by not creating a new object when some objects are repeated, but still uses the original object. This reused object is called a shared object. Currently, shared objects only support string objects with integer values.

The concrete implementation of shared objects

Redis' shared objects currently only support string objects with integer values. This is actually a balance between memory and CPU (time): while shared objects reduce memory consumption, it takes extra time to determine if two objects are equal. The complexity of the operation is O(1) for integer values; O(n) for ordinary strings; and O(n^2) for hashes, lists, collections, and ordered collections.

While shared objects can only be string objects with integer values, shared objects may be used for all five types (e.g., elements of hashes, lists, etc. can be used).

In the current implementation, the Redis server creates 10,000 string objects with integer values from 0 to 9999 at initialization; when Redis needs to use string objects with values from 0 to 9999, it can use these shared objects directly. 10,000 can be adjusted with the parameter REDIS_SHARED_INTEGERS (in 4.0 it was OBJ_SHARED_INTEGERS) value to change.

The number of references to a shared object can be viewed with the object refcount command, as shown in the following figure. The result page of the command confirms that only integers between 0 and 9999 are shared.

![](8.jpg)

(5) ptr
The ptr pointer points to the specific data, as in the previous example, set hello world, and ptr points to the SDS containing the string world.

(6) Summary
In summary, the structure of a redisObject is related to object type, encoding, memory recycling, and shared objects; a redisObject object has a size of 16 bytes.

4bit+4bit+24bit+4Byte+8Byte=16Byte.

4. SDS
Instead of using C strings (i.e., arrays of characters ending with the null character '\0') as the default string representation, Redis uses SDS, which is an acronym for Simple Dynamic String.

(1) SDS structure

![](9.jpg)

(2) Comparison of SDS and C strings
The addition of the free and len fields to the C string by SDS brings a number of benefits.

Get string length: SDS is O(1), C string is O(n)
Buffer overflow: When using the API for C string, if the string length increases (e.g., strcat operation) and memory is forgotten to be reallocated, it is easy to cause a buffer overflow; while SDS records the length, the corresponding API will automatically reallocate memory when it may cause a buffer overflow, thus eliminating buffer overflow.
Memory reallocation when modifying a string: For C string, if you want to modify the string, you must reallocate memory (release first and then apply), because if you don't reallocate, the memory buffer overflow will be caused when the string length increases, and the memory leak will be caused when the string length decreases. As for SDS, since len and free can be recorded, the correlation between string length and space array length is relieved, and can be optimized on this basis: the space pre-allocation policy (i.e., allocating more memory than actually needed) reduces the probability of reallocating memory when string length increases; the inert space release policy reduces the probability of reallocating memory when string length decreases. Significantly reduced.
Accessing binary data: SDS is OK, C string is not. Because the C string identifies the end of a string as a null character, and for some binary files (such as images), the contents may include an empty string, so the C string cannot be accessed correctly; SDS identifies the end of a string as a string length len, so there is no such problem.
In addition, since the buf in SDS still uses the C string (i.e. ends with '\0'), SDS can use some of the functions in the C string library; however, it should be noted that it can only be used when SDS is used to store text data, but not when storing binary data ('\0' is not necessarily the end of the string).

(3) Application of SDS to C-strings
Redis always uses SDS instead of C string when storing objects. For example, with the set hello world command, hello and world are both stored as SDS. The sadd myset member1 member2 member3 command stores both the key ("myset") and the elements in the set ("member1", "member1"). "member2" and "member3"), both of which are stored in the form of SDS. In addition to storing objects, SDS is also used to store various buffers.

The C string is only used if the string does not change, such as when printing a log.

