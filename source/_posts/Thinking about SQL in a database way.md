---
title: Thinking about SQL in a database way
date: 2020-09-27 19:07:35
tags: SQL
category: SQL
---
## How SQL is executed in Oracle

![](1.jpg)

1. Syntax Check: Check if the SQL spelling is correct, if not, Oracle will report syntax error.

2. Semantic check：Check if the access object in SQL exists. For example, when we write SELECT statement, the column name is written wrongly, the system will report an error. The purpose of syntax check and semantic check is to make sure there are no errors in the SQL statement.

3. Permissions check: to see if the user has permission to access the data.

4. The shared pool check: the shared pool is a memory pool, the main purpose is to cache the SQL statements and the execution plan of the statements, Oracle by checking whether the shared pool exists the execution plan of the SQL statements, to determine the soft or hard parsing. What do you mean by soft parsing and hard parsing?

In the shared pool, Oracle first performs a Hash operation on the SQL statements, and then looks for them in the library cache according to the Hash value, if there is an execution plan for the SQL statements, it will be used to execute them directly, and directly enter the "executor" link, which is soft parsing.

If no SQL statement and execution plan are found, Oracle needs to create a parse tree for parsing, generate an execution plan, and enter the "optimizer" step, which is hard parsing.

4. Optimizer: the optimizer is to do hard parsing, that is, to decide what to do, such as creating the parse tree and generating the execution plan.

5. Executor: When you have a parse tree and execution plan, you know how the SQL should be executed, so you can execute statements in the executor.

Shared pool is a term used in Oracle that includes library cache, data dictionary buffer, etc. We have already talked about the library buffer above, which mainly caches SQL statements and execution plans. And the data dictionary buffer stores object definitions in Oracle, such as tables, views, indexes, and other objects. When parsing SQL statements, if relevant data is needed, it is fetched from the data dictionary buffer.

This one step, the library cache, determines whether the SQL statement needs to be hard parsed or not. To improve the efficiency of SQL execution, we should avoid hard parsing as much as possible, because creating parse trees and generating execution plans during SQL execution is resource consuming.

You may ask, how to avoid hard parsing and use soft parsing as much as possible? In Oracle, bind variables are one of its major features. Binding variables is the use of variables in a SQL statement to change the result of SQL execution by taking different values of the variable. The advantage of this is that it improves the possibility of soft parsing, but the disadvantage is that it may result in an unoptimized execution plan, so the need to bind variables depends on the situation.

As an example, we could use the following query statement.

`SQL> select * from player where player_id = 10001;`

You can also use binding variables such as.

`SQL> select * from player where player_id = :player_id;`

The efficiency of these two query statements is completely different in Oracle. If you also query 10002, 10003, and so on after querying player_id = 10001, then a new query parse is created for each query. Whereas the second way uses bind variables, then after the first query, the execution plan for this type of query will exist in the shared pool, i.e., soft parsing.

So we can reduce the hard parsing and Oracle's parsing workload by using bind variables. However, there are disadvantages to this approach, using dynamic SQL will lead to different SQL execution efficiency and also SQL optimization will be difficult because of different parameters.

## How SQL is executed in MySQL

![](573.jpg)

Oracle uses a shared pool to determine if the SQL statement has a cache and execution plan, and we can tell whether we should use hard or soft parsing from this step. So how does SQL get executed in MySQL?

First of all, MySQL is a typical C/S architecture, i.e. Client/Server architecture, and server-side programs use mysqld.

MySQL consists of three layers.

1. Connection layer: client and server side establish connection, client sends SQL to server side.
2. SQL layer: query processing of SQL statements.
3. Storage engine layer: deals with database files and is responsible for storing and reading data.

The flow of SQL statements in MySQL is: SQL statement → cache query → parser → optimizer → executor. In part, MySQL and Oracle execute SQL in the same way.

1. Query Cache: If the Server finds this SQL statement in the query cache, it will return the result directly to the client; if not, it will go to the parser stage. It should be noted that this feature has been abandoned after MySQL 8.0 because the query cache is often inefficient.
2. Parser: The parser performs syntactic analysis and semantic analysis on SQL statements.
3. Optimizer: In the optimizer, the execution path of SQL statements is determined, such as whether to search according to the whole table or index.
4. Executor: Determine whether the user has privileges before execution, and if so, execute the SQL query and return the result. In MySQL versions below 8.0, if the query cache is set, the query result will be cached.

Unlike Oracle, MySQL's storage engines are in the form of plug-ins, each of which is geared towards a specific database application environment. The open source MySQL also allows developers to set up their own storage engines, and the following are some common ones.

1. InnoDB Storage Engine: It is the default storage engine after MySQL 5.5 version, the biggest feature is that it supports transaction, row-level locking, foreign key constraint, and so on.
2. MyISAM Storage Engine: It is the default storage engine before MySQL 5.5, and does not support transactions or foreign keys.
3. Memory storage engine: It uses system memory as the storage medium to get faster response speed. However, if mysqld process crashes, all the data will be lost, so we only use the Memory storage engine when the data is temporary.
4. NDB Storage Engine: Also called NDB Cluster Storage Engine, mainly used for MySQL Cluster distributed cluster environment, similar to Oracle's RAC cluster.
5. Archive storage engine: It has a good compression mechanism for archiving files and compresses them when a write request is made, so it is also often used as a repository.

It is important to note that the design of the database lies in the design of the tables, and each table in MySQL can be designed with a different storage engine, and we can choose the storage engine based on the actual data processing needs, which is what makes MySQL so powerful.

## A database management system is also a type of software
Since a single SQL statement goes through different modules, let's look at the resources (time) used for SQL execution in the different modules. I'll show you how to analyze the execution time of a SQL statement in MySQL.

First we need to see if profiling is enabled, enabling it allows MySQL to collect information about the resources used during SQL execution, with the following command.

`mysql> select @@profiling;`

![](11.jpg)

`profiling=0` means off, we need to turn profiling on, i.e. set to 1.

`mysql> set profiling=1;`

Then we execute a SQL query

`mysql> select * from wucai.heros;`

View all profiles generated by the current session.

![](22.jpg)

You will notice that we have just executed two queries with Query IDs of 1 and 2. If we want to get the execution time of the last query, we can use.

`mysql> show profile;`

![](33.jpg)

Of course you can also query for a specific Query ID, such as.

`mysql> show profile for query 2;`

The result of querying SQL execution time is the same as above.

After version 8.0, MySQL no longer supports cached queries for the reasons I mentioned above. The cache is emptied as soon as a table is updated, so it's only worth using cached queries if the table is static, or if the table has changed very little, otherwise it will increase the query time for SQL if the table is updated frequently.

You can use select version() to see the version of MySQL.

![](44.jpg)

## Sum up
We tend to see the forest for the trees when working with SQL and don't notice how it performs in the various database software, but today we're going to understand this from a full picture perspective. You can see the similarities and differences between the different RDBMSs.

The similarity is that both Oracle and MySQL execute SQL through the process of Parser→Optimizer→Executor.

Oracle proposes the concept of shared pool to determine whether to do soft parsing or hard parsing. In MySQL, version 8.0 onwards no longer supports query caching, but directly executes the process of parser→optimizer→executor, which can also be seen in the show profile of MySQL. In addition, MySQL provides a variety of storage engines to choose from, and different storage engines have their own usage scenarios.
