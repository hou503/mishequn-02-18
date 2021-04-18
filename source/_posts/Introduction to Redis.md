---
title: Introduction to Redis
date: 2020-11-01 18:02:21
tags: Redis
category: Redis
---
## What is Redis and why so soon?
Redis is called REmote DIctionary Server, and you can see from the name that it uses a dictionary structure to store data, that is, key-value type data.

According to the official data, Redis can process up to 100,000 requests per second.

Why is it so fast?

Redis is written in ANSI C, which is the same language as SQLite. The advantage of writing in C is that the underlying code execution is efficient and has few dependencies, since libraries developed in C do not have many runtime dependencies, and the system is compatible and stable.

In addition, Redis is a memory-based database, which, as we've talked about before, avoids disk I/O, hence Redis is also known as a caching tool.

Redis uses Key-Value to store the data, which means it uses a hash structure to operate, and the complexity of data operation is O(1).

It uses a single-process, single-threaded model, which has the advantage of avoiding context switching and unnecessary resource competition between threads.

Technically Redis also uses multiple I/O multiplexing. Here, multiplexing refers to multiple socket network connections, and multiplexing refers to multiplexing the same thread. The advantage of multiple I/O multiplexing is that multiple I/O requests can be handled in the same thread, minimizing network I/O consumption and increasing efficiency.

## Redis Data Types
The data types supported by Redis include strings, hashes, lists, collections, ordered collections, etc. The most basic data types provided by Redis are strings, hashes, lists, collections, and ordered collections.

The string type is the most basic data type provided by Redis, and the corresponding structure is key-value.

If we want to set the value of a key, we can use set key value, for example, if we want to set the value of name to test, we can write set name test, if we want to get the value of a key, we can use get key, for example, if we want to get the value of name, we can write get name.

A hash provides a mapping between fields and field values, and the corresponding structure is key-field-value.

If we want to set the hash value of a key, we can use hset key field value, and if we want to set username to t and age to 28 for user1, we can write something like this.

```sql
hset user1 username zhangfei
hset user1 age 28
```
If you want to get the value of a field for a key, you can use hget key field, for example, if you want to get the username of user1, then just write `hget user1 username`.

If we want to set multiple field-values to a key key at the same time, we can use hmset key field value [field value...] For example, the one above could be written as.

```sql
Hmset user1 username zhangfei age 28
```
If you want to get the value of a field for a key, you can use hget key field, e.g. if you want to get the username of user1, then just write hget user1 username.

If you want to get multiple field values for a key at once, you can use hget key field[field...]. If you want to get the username and age of user1, for example, you can write hmget user1 username age.

![](1.jpg)

Underlying the list is a two-way chain structure, so we can add elements to both ends of the list with O(1) time complexity, and we can also retrieve a fragment of the list.

If we want to add elements to the left side of the list we can use: LPUSH key value [...] For example, if we add zhangfei, guanyu and liubei elements to the left side of the heroList, we can write.

```sql
LPUSH heroList zhangfei guanyu liubei
```
Similarly, we can also use the RPUSH key value [...] To add elements to the right side of the list, for example, if we add two elements dianwei and lvbu to the right side of the heroList list, we can write something like this.

```sql
RPUSH heroList dianwei lvbu
```
If we want to get the content of a segment in the list, just use the LRANGE key start stop, e.g. if we want to get the heroList from 0 to 4 position, just write LRANGE heroList 0 4.

![](2.jpg)

A set is an unordered collection of strings. It differs from a list in that the elements in the collection are unordered and the elements cannot be duplicated.

If you want to add elements to a set, you can use the SADD key member [...] For example, if we add the elements zhangfei, guanyu, libubei, dianwei and lvbu to our heroSet collection, we can write.

```sql
SADD heroSet zhangfei guanyu liubei dianwei lvbu

```
If you want to remove an element from a collection, you can use the SREM key member [...] For example, if we want to remove the elements liubei and lvbu from the heroSet collection, we can write.
```sql
SREM heroSet liubei lvbu
```

If we want to get all the elements in the set, we can use the SMEMBERS key, e.g. if we want to get all the elements in the heroSet set, we can write SMEMBERS heroSet.

If we want to determine if an element exists in the set, we can use the SISMEMBERS key member, e.g. if we want to determine if zhangfei and liubei exist in the heroSet set, we can write SMEMBERS as follows.

```sql
SISMEMBER heroSet zhangfei
SISMEMBER heroSet liubei
```
![](3.jpg)

We can understand an ordered string collection as an upgrade to a collection. In fact, ZSET adds a score attribute to the set, which can be specified when you add a modified element. Each time it is specified, ZSET is automatically sorted by score, which means that we can specify the score when we add a member to the set key.

An ordered collection has some similarities to a list, in that both data types are ordered and both have access to a range of elements. But it is very different in the data structure of the two, the first list list is achieved by a two-way chain table, in the operation of the left and right side of the data will be very fast, and for the middle of the data operation is relatively slow. Ordered collection using the hash table structure to achieve, read sorted in the middle part of the data will also be fast. Also the ordered set can be adjusted by score, but if we want to adjust the position of the elements of the list, it is more difficult.

If we want to add elements and scores to an ordered set, use the ZADD key score member [...] Let's say we add the hp_max values of the following 5 heroes to the heroScore collection, as shown in the following table.

![](4.jpg)

Then we could write something like this.
```sql
ZADD heroScore 8341 zhangfei 7107 guanyu 6900 liubei 7516 dianwei 7344 lvbu
```
If we want to get the score of an element, we can use the ZSCORE key member, e.g. we want to get the score of guanyu, just write ZSCORE heroScore guanyu.

If we want to remove one or more elements, we can use ZREM key member [member ...], for example if we want to remove the element guanyu, just use ZREM heroScore guanyu.

We can also get a list of elements in a range. Use the ZRANGE key start stop [WITHSCORES] if you want to sort the scores from smallest to largest, or the ZREVRANGE key start stop [WITHSCORES] if you want to sort the scores from largest to smallest. It should be noted that WITHSCORES is optional, if you use WITHSCORES it will show the scores together, e.g. if we want to find the top 3 heroes with scores in the ordered set heroScore and their values, write ZREVRANGE heroScore 0 2 WITHSCORES! .

![](5.jpg)

In addition to these five data types, Redis also supports Bitmaps data structures, adding HyperLogLog after version 2.8, Geospatial and index radius queries after version 3.2, and Streams data type.

## How to use Redis
We can manipulate Redis directly in Python, and we need to use the pip install redis install kit to reference it before using it, once installed, we need to use import redis before using it.

There are two ways to connect to Redis in Python, the first of which is directly, using the following line of commands.
```python
r = redis.Redis(host='localhost', port= 6379)
```

The second is the connected pool approach.
```python
pool = redis.ConnectionPool(host='localhost', port=6379)
r = redis.Redis(connection_pool=pool)
```
Normally, when we connect to Redis, we can create a Redis connection, perform Redis operations through it, and then release it when it's done. However, in a high concurrency situation, this is very uneconomical because each connection and release consumes a lot of resources.
## Why use a connection pool mechanism
Redis provides a connection pooling mechanism that allows us to create multiple connections in advance, place them in the connection pool, and fetch them directly from the connection pool when we need to perform Redis operations.

The connection pooling mechanism eliminates the need to create and release connections, improving overall performance.
## Principles of the connection pool mechanism

n the instance of the connection pool there will be two lists, held in _available_connections and _in_use_connections, which represent the set of connections that are available in the connection pool and the set of connections that are in use, respectively. When we want to create a connection, we can get a connection from _available_connections to use and put it in _in_use_connections. If there are no available connections, only then will we create a new connection and place it in _in_use_connections. If the connection is exhausted, it is removed from _in_use_connections and added to _available_connections for subsequent use.

The Redis library provides the Redis and StrictRedis classes, which both implement Redis commands, with the difference that Redis is a subclass of StrictRedis and is compatible with older versions. If we wanted to use the connection pooling mechanism and then instantiate it with StrictRedis, we could write something like this.


```python
import redis 
pool = redis.ConnectionPool(host='localhost', port=6379)
r = redis.StrictRedis(connection_pool=pool)
```
## Sum up
In practice, we often use RDBMS and Redis together to complement each other.

As a common NoSQL database, Redis supports much richer data types than Memcached. In terms of I/O performance, Redis adopts a single-threaded I/O multiplexing model, while Memcached is multi-threaded and can take advantage of multi-core. In terms of persistence, Redis provides two persistence modes, which can keep data permanently, which is not available in Memcached.

