---
title: Introduction to Redis(3)
date: 2020-11-21 21:24:11
tags: Redis
category: Redis
---

## Redis object types and internal coding

Redis supports five object types, and each structure has at least two encodings; this has the advantage of separating the interface from the implementation, so that when the internal encoding needs to be added or changed, the user is not affected, and on the other hand, the internal encoding can be switched according to different scenarios to improve efficiency.

With respect to the conversion of Redis internal encodings, the following rule applies: the encoding conversion is done when Redis writes data, and the conversion process is not reversible, only small-memory encodings can be converted to large-memory encodings.

1. String
(1) Overview
Strings are the most basic type, since all keys are strings and several other elements of complex types other than strings are also strings.

The length of a string cannot exceed 512 MB.

(2) Internal encoding
There are three types of internal coding for string types and they are used in the following scenarios.

int: 8 bytes long integer. When the string value is integer, the value is represented by a long integer.
embstr: <= 39 bytes string. embstr and raw both use redisObject and sds to store data, the difference is that embstr uses only one memory allocation (so redisObject and sds are continuous), while raw needs two memory allocations (redisObject and (sds allocates space). So the advantages of embstr over raw are less space allocated once when creating, less space released when deleting, and the ease of finding all of the object's data linked together. The downside of embstr is also obvious: if the length of a string increases and memory needs to be reallocated, the entire redisObject and sds will need to be reallocated, so embstr is implemented as read-only in redis.
raw: strings larger than 39 bytes
An example is shown in the figure below.

![](1.jpg)
The length of embstr and raw to distinguish is 39; because the length of redisObject is 16 bytes, and the length of sds is 9 + string length; therefore, when the string length is 39, the length of embstr is exactly 16 + 9 + 39 = 64, and jemalloc can allocate exactly 64 bytes of memory units.

(3) encoding conversion
When the int data is no longer an integer, or the size exceeds the range of the long, it is automatically converted to raw.

As for embstr, because its implementation is read-only, when the embstr object is modified, it will be converted to raw first and then modified, so as long as the embstr object is modified, the modified object must be raw, regardless of whether it reaches 39 bytes. An example is shown in the following figure.

![](2.jpg)

2. List
(1) Overview
Lists are used to store multiple ordered strings, each string is called an element; a list can store 2^32-1 elements.Lists in Redis support insertion and popup at both ends, and can get elements at a specified location (or range), and can act as arrays, queues, stacks, and so on.

(2) Internal coding
The internal encoding of the list can be either a ziplist or a linkedlist.

A double-ended linked list consists of a list structure and multiple listNode structures; a typical structure is shown in the following figure.

![](3.jpg)

As you can see from the diagram, the double-ended chain table holds both header and tail pointers, and each node has pointers to the front and back; the length of the list is saved in the chain table; and dup, free, and match set type-specific functions for the node values, so the chain table can be used to save a variety of different types of values. And each node in the chain table points to a redisObject of type string.

 

Compressed list: Compressed list is a sequential data structure consisting of a series of specially coded sequential blocks (instead of a double-ended table where each node is a pointer) to save memory. Compared with double-ended chain table, compressed list can save memory space, but the complexity is higher when modify or add or delete operations; therefore, when the number of nodes is small, you can use compressed list; but when the number of nodes is large, it is cost-effective to use double-ended chain table.

The compressed list is not only used to implement the list, but also used to implement the hash, ordered list; very widely used.

(3) encoding conversion
A compressed list is used only if both of the following two conditions are met: the number of elements in the list is less than 512; and all string objects in the list are less than 64 bytes. If one of the conditions is not met, a double-ended list is used; and encoding is only possible from a compressed list to a double-ended chained list, not the other way around.

The following diagram illustrates the characteristics of list encoding conversion.
![](4.jpg)

where a single string cannot exceed 64 bytes to facilitate a uniform distribution of the length of each node; here, 64 bytes is the length of the string, excluding the SDS structure, because compressed lists use continuous, fixed-length blocks of memory to store strings and do not require the SDS structure to specify the length. The compressed list mentioned later will also emphasize the length of no more than 64 bytes, similar to the principle here.

3、Hash
(1) Overview
A hash (as a data structure) is not only one of the five object types provided by redis (along with strings, lists, collections, and ordered combinations), it is also the data structure used by Redis as a Key-Value database. For ease of explanation, when the term "inner hash" is used later in this document, it refers to one of the five object types provided by redis; when the term "outer hash" is used, it refers to the data structure used by Redis as the Key-Value database. The data structure.

(2) Internal coding
The inner hash uses an internal code that can be either a ziplist or a hashtable; the outer hash of Redis uses only the hashtable.

The compressed list was described earlier. Compared to the hashtable, the compressed list is used for scenarios where the number of elements is small and the length of the elements is small; its advantage is that it is stored centrally, which saves space; at the same time, although the complexity of the operations on the elements is changed from O(1) to O(n), there is no significant disadvantage in the time required for the operations because the number of elements in the hash is small.

 

hashtable: a hashtable consists of a dict structure, two dictht structures, an array of dictEntry pointers (called a bucket), and multiple dictEntry structures.

The relationship between the parts under normal circumstances (i.e., when the hashtable is not rehashing) is shown in the following diagram.

![](5.jpg)

The following describes the components in order from the bottom up.

dictEntry

The dictEntry structure is used to hold key-value pairs and is defined as follows.

![](6.jpg)

The functions of each of these attributes are as follows.

key: the key of the key-value pair.
val: the value in a key-value pair, implemented using union (i.e., a common body), which may store either a pointer to a value, a 64-bit integer, or an unsigned 64-bit integer.
next: point to the next dictEntry for hash conflict resolution
On 64-bit systems, a dictEntry object takes up 24 bytes (8 bytes each for key/val/next).

dictEntry

The size of the bucket array in redis is calculated according to the following rules: greater than dictEntry, the smallest 2^n; for example, if there are 1000 dictEntry, then the bucket size is 1024; if there are 1500 dictEntry, then the bucket size is 2048.

dictht

The dictht structure is as follows.

![](7.jpg)

The function of each of these attributes is described below.

The table attribute is a pointer to the bucket.
The size attribute records the size of the hash table, i.e., the size of the bucket.
used records the number of dictEntry's that have been used.
The value of the sizemask attribute is always size-1. This attribute, along with the hash value, determines where a key is stored in the table.
sizemask

In general, by using the dictht and dictEntry structures, it is possible to implement the functionality of a normal hash table; however, in the Redis implementation, there is a dict structure on top of the dictht structure. The following is the definition and function of the dict structure.

The dict structure is as follows.
![](8.jpg)


The type attribute and the privdata attribute are used to accommodate different types of key-value pairs and are used to create polymorphic dictionaries.

The ht attribute and the trehashidx attribute are used for rehash, i.e., when the hash table needs to be expanded or contracted. ht is an array containing two items, each of which points to a dictht structure, which is why Redis hashes will have one dict and two dictht structures. Normally, all the data is in ht[0] of the put dict, and ht[1] is only used in the rehash. dict does the rehash operation by rehashing all the data in ht[0] into ht[1]. It then assigns ht[1] to ht[0] and empties ht[1].

Thus, the reason why the hash in Redis has a dict structure in addition to the dictht and dictEntry structure is to accommodate different types of key-value pairs on the one hand and rehash on the other.

(3) Encoding conversion
As mentioned earlier, the inner hash in Redis may use either a hash table or a compressed list.

A compressed list is used only if both of the following two conditions are met: the number of elements in the hash is less than 512; and the length of the key and value strings for all key-value pairs in the hash is less than 64 bytes. If one of the conditions is not met, the hash table is used; and the encoding can only be converted from a compressed list to a hash table, not the other way around.

The following diagram illustrates the features of the hash encoding conversion within Redis.

![](9.jpg)

4. Assembly
(1) Overview
A set is similar to a list in that it is used to store multiple strings, but there are two differences between a set and a list: the elements in a set are unordered, so you can't manipulate the elements by indexing them; and the elements in a set can't be duplicated.

A collection can store up to 2^32-1 elements; in addition to the usual add/drop support, Redis also supports multiple sets of intersection, concatenation, differential set.

(2) Internal coding
The internal encoding of a collection can be either an inset or a hashtable.

The hash table has already been discussed, so I'll skip it here; it should be noted that when you use a hash table, the values are all set to null.

The structure of an integer set is defined as follows.
![](10.jpg)
The encoding represents the type of content stored in the content. Although the content is of type int8_t, it actually stores the value of int16_t, int32_t or int64_t, which is determined by encoding; length represents the number of elements.

The integer set is suitable for when all elements of the set are integers and the number of elements in the set is small, compared to a hash table, the advantage of integer sets is centralized storage, saving space; at the same time, although the complexity of the operation of the elements from O (1) to O (n), but due to the smaller number of sets, the operation of the time is not a significant disadvantage.

(3) Code conversion
Sets use integer sets only if both of the following conditions are met: the number of elements in the set is less than 512; all elements in the set are integer values. If one of the conditions is not satisfied, a hash table is used; and the encoding can only be converted from an integer set to a hash table, not the other way around.

The following diagram illustrates the characteristics of the set encoding conversion.
![](11.jpg)

5. Orderly assembly
(1) Overview
An ordered set is the same as a set, the elements cannot be duplicated; however, unlike a set, the elements in an ordered set are ordered. With the list of index subscript as the basis for sorting different, ordered collection for each element to set a score (score) as the basis for sorting.

(2) Internal coding
The internal encoding of an ordered collection can be either a compressed list (ziplist) or a jump table (skiplist). ziplist is used in both lists and hashes and has already been discussed, so I'll skip it here.

A jump table is an ordered data structure that enables fast access to nodes by maintaining multiple pointers to other nodes in each node. In addition to jump tables, another typical implementation of an ordered data structure is a balanced tree; in most cases, the efficiency of jump tables is comparable to that of balanced trees, and they are much simpler to implement than balanced trees, so they are used instead of balanced trees in redis. It supports the average O(logN) and worst O(N) complex points for node lookup, and also supports sequential operations. The specific structure is relatively complex, slightly.

(3) Code Conversion
A compressed list is used only if both of the following two conditions are met: the number of elements in the ordered set is less than 128; and all members of the ordered set are less than 64 bytes in length. If one of these conditions is not met, a jump table is used; and encoding is only possible from a compressed list to a jump table, not the other way around.






