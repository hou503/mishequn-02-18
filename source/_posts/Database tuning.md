---
title: Database tuning
date: 2020-10-17 20:24:50
tags: Database tuning
category: Database tuning
---
## Objectives of database tuning
Simply put, the goal of database tuning is to make the database run faster, which means faster response time and greater throughput.

However, with the increasing number of users and the increasing complexity of applications, it is difficult to define the goal of database tuning in terms of "faster" because users encounter different bottlenecks when accessing the server at different times, such as the Double Eleven sale, which brings massive concurrent access; and users who are conducting different business operations. The database transaction processing and SQL queries will be different at the time of tuning. Therefore, we also need to be more fine-grained to determine the goals of tuning.

How do we do this? In general, there are two ways to get feedback.

### Feedback from users
Users are the ones we serve, so their feedback is the most direct. While they don't make direct technical suggestions, some problems are often the first thing users notice. We need to take user feedback seriously and find issues that are relevant to the data.

### Log analysis
We can identify anomalies by looking at database logs and operating system logs, etc., and use them to locate problems encountered.

In addition to these specific feedbacks, we can also get an overall view of the server and database performance by monitoring the operational status.

### Server Resource Usage Monitoring
By monitoring the server's CPU, memory, I/O and other usage, you can understand the server's performance in real time and compare it with the historical situation.

### Database Internal Status Monitoring
In database monitoring, active session monitoring is an important indicator. With it, you can see if the database is currently very busy, if there are SQL stacks, etc. In addition to active session monitoring, we can also monitor transactions, lock waits, etc. This helps us to have a more comprehensive view of the database running.

In addition to active session monitoring, we can also monitor transactions, lock waits, etc., which can help us get a more complete picture of the database's performance.

## What are the dimensions that can be selected for tuning the database?
What we need to tune is the entire database management system, including not only SQL queries, but also database deployment configuration, architecture, etc. From this perspective, we think about more than just SQL optimization. From this point of view, we are not limited to the dimension of SQL optimization.

It sounds complicated, but we can actually go through it step by step with the following steps.

### Step 1: Select the right DBMS
We've talked about the SQL camp and the NoSQL camp before. In RDBMS, the common ones are Oracle, SQL Server and MySQL. If you have high requirements for transactional processing and security, you can choose commercial database products. These databases are strong in transaction processing and query performance, such as using SQL Server, then a single table to store hundreds of millions of data is no problem. If the data tables are well-designed, the query efficiency is not bad even if you don't use a library-by-table approach.

In addition, you can also use the open-source MySQL for storage, which, as we have mentioned before, has many storage engines to choose from, including InnoDB for transactional processing and MyISAM for non-transactional processing.

The NoSQL camp includes key-value databases, document-based databases, search engines, columnar stores, and graph databases. These databases have different advantages and disadvantages and different usage scenarios. For example, columnar storage databases can greatly reduce system I/O and are suitable for distributed file systems and OLAP, but if data needs to be frequently added and deleted, columnar storage is not suitable. I've already explained the reasons in the Q&A section, so I won't repeat them here.

The choice of DBMS affects the whole design process, so the first step is to choose the right DBMS, if you have already decided on a DBMS, then this step can be skipped, but sometimes we have to choose according to the business needs.
### Step 2: Optimize table design
After selecting the DBMS, we need to do the table design.In RDBMS, each object can be defined as a table, and the relationship between the tables represents the relationship between the objects. If we are using MySQL, we can also choose a different storage engine depending on the usage requirements of different tables. In addition to this, there are some principles of optimization that can be followed.

The table structure should follow the principles of the third paradigm as much as possible. This makes the data structure clearer and more standardized, reduces the number of redundant fields, and also reduces the occurrence of exceptions when updating, inserting, and deleting data.
If there are a lot of analytical queries, especially if you need to perform multi-table lookups, you can use the inverse paradigm for optimization. The inverse paradigm uses a space-for-time approach to improve the efficiency of the query by adding redundant fields.
The choice of the data type of the table field is related to the efficiency of the query and the size of the storage space. In general, if the field can be of numeric type do not use character type; character length should be designed to be as short as possible. For the character type, when determining the character length is fixed, you can use the CHAR type; when the length is not fixed, usually using the VARCHAR type.
The structure of the data table design is very basic, but also very critical. Good table structure can still work in the case of business development and user volume increase, bad table structure design will make the data table becomes very bloated, query efficiency will be reduced.

### Step 3: Optimize logical queries
Once we have created the data table, we can then perform the add/drop operations on the data table. The first thing we need to consider at this point is logical query optimization, what is logical query optimization?

SQL query optimization can be divided into logical query optimization and physical query optimization. Logical query optimization is the process of making SQL execution more efficient by changing the contents of SQL statements, using an equivalent transformation of SQL statements to rewrite the query. The mathematical basis for query rewriting is relational algebra.

Query rewriting for SQL includes subquery optimization, equivalence predicate rewriting, view rewriting, conditional simplification, join elimination, and nested join elimination.

For example, when we explain EXISTS subqueries and IN subqueries, we will select the appropriate subqueries based on the principle of small table driving large tables. In the WHERE subquery, we will try to avoid functional operations on fields, they can invalidate the index of the field.

### Step 4: Optimize physical queries
Physical query optimization is the process of turning the contents of logical queries into physical operators that can be executed in preparation for the subsequent execution of the executor. It is all about efficiently creating indexes and using these indexes to do various optimizations.

But you should know that indexes are not everything and we need to create them according to the actual situation. So what are the situations that need to be considered?

If the data has a high degree of repetition, you do not need to create an index. Usually, if the repetition level is more than 10%, you may not create an index for this field.
Be aware of how the position of the index column affects the use of the index. For example, if we calculate an expression for an index field in the WHERE clause, it will cause the index of this field to be invalid.
Be aware of the effect of joint indexes on index usage. When we create a union index, we create indexes for multiple fields, and the order of the indexes is important. For example, if we create indexes for fields x, y, z, there will be a difference in the order of (x,y,z) or (z,y,x) at execution time.
Be aware of the effect of multiple indexes on index usage. The more indexes you have, the better, because each index requires storage space, and more indexes means more storage space is required. In addition, too many indexes can cause the optimizer to increase the computation time to filter out the indexes, which can affect the efficiency of the evaluation.
After equivalence transforming the SQL statement, the query optimizer also needs to determine the access path based on the index and data condition of the data tables, which determines the resources consumed when executing SQL. different data tables are required to be queried during SQL queries, so the physical query optimization phase also needs to determine the paths used by these queries, in the following cases.

Single table scan: for a single table scan, we can scan all the data in the whole table or partial scan.
Joining of two tables: common joining methods include nested loop joins, HASH joins and merge joins.
Joining multiple tables: when joining multiple data tables, the order is important because the query efficiency and search space will be different for different connection paths. When we are making multi-table joins, the search space may reach very high data levels, and the huge search space will obviously consume more resources, so we need to adjust the search space to an acceptable range by adjusting the order of the joins.
Physical query optimization is a technique that uses physical optimization after determining the logical query optimization to estimate the various possible access paths by computing the cost model to find the least costly of the execution methods as the execution plan. In this section, we need to master the focus on the creation and use of indexes.

### Step 5: Use Redis or Memcached as a cache
In addition to optimizing the SQL itself, we can also hire outside help to improve the efficiency of the queries.

Since the data is stored in the database, we need to take out the data from the database layer and put it into memory for business logic operations, and when the number of users increases, if we query the data frequently, it will consume a lot of resources in the database. If we put the frequently used data directly into the memory, we can improve the efficiency of the query greatly.

A key-value store database can help us solve this problem.

Commonly used key-value storage databases are Redis and Memcached, both of which can store data in memory.

In terms of reliability, Redis supports persistence, which allows us to keep our data on the hard drive, but this can also be quite performance intensive. Memcached, on the other hand, is only in-memory storage and does not support persistence.

It supports not only key-value data, but also List, Set, Hash and other data structures. We can use Redis when we have persistence or more advanced data processing needs, or Memcached for simple key-value storage.

Usually, we can consider in-memory databases for scenarios with high query response requirements (short response time, high throughput). Traditional RDBMSs store data on the hard drive, while in-memory databases are stored in memory and are much faster to query. Using different tools, however, also increases the cost of use for developers.

### Step 6, library-level optimization
Library level optimization is an optimization strategy that stands on the dimension of the database, such as controlling the number of data tables in a library. Alternatively, we can use a master-slave architecture to optimize our read and write strategies.

If the read and write business are very large, and they are all operating in the same database server, then the database performance will appear bottleneck, in order to improve the system performance and optimize the user experience, we can use the read and write separation to reduce the load of the master database, such as using the master database (master) to complete the write operation, use the database (slave) to complete the read operation.

In addition to this, we can also sub-database sub-tables. When the amount of data reaches billions, sometimes we need to cut a database into multiple copies and put them on different database servers to reduce the access pressure to a single database server. If you are using MySQL, you can use the partitioned table feature that comes with MySQL, of course you can also consider doing vertical and horizontal slicing yourself.

When to do a vertical slice and when to do a horizontal slice?

If there are too many data tables in the database, you can use vertical splitting to deploy the associated data tables on a single database.

If there are too many columns in the data table, you can use vertical slicing to split the data table into multiple sheets and put the columns that are often used together into the same table.

If the data table reaches billions or more, you can consider horizontal slicing, splitting the large data table into different sub-tables, keeping the same table structure for each table. For example, you can split the data by year and put the data from different years into different data tables. 2017, 2018, and 2019 data can be put into three separate data tables.

With vertical splitting, you are splitting a single data table into multiple tables, and with horizontal splitting, you are splitting a single data-heavy table into different smaller tables according to a certain attribute dimension.

However, it is important to note that splitting increases the cost of maintenance and usage while improving database performance.

How do we think about and analyze this database tuning thing?
Before we do anything, we need to identify our goals. In database tuning, our goals are faster response times and greater throughput. Using macro monitoring tools and micro log analysis can help us quickly find ideas and ways of tuning.

While everyone's situation is different, we also need to have a holistic view of the database tuning thing. When thinking about database tuning, there are three dimensions to consider.

First, choice is more important than effort.

Before you do SQL tuning, you can choose how the DBMS and data tables are designed. As you can see, the different DBMS directly determines how the subsequent operations will be done, and the way the data tables are designed directly affects the subsequent SQL query statements.

In addition, you can divide SQL query optimization into two parts, logical query optimization and physical query optimization.

Although there are many techniques for SQL query optimization, the general direction can be completely divided into two major parts, logical query optimization and physical query optimization. Logical Query Optimization is the process of improving the efficiency of a query through SQL Equivalent Transformation, which to be blunt, means that the query may be written in a different way and executed more efficiently. Physical query optimization, on the other hand, is done through techniques such as indexes and table joins, where the focus is on mastering the use of indexes.

Finally, we can enhance the performance of the database with external help.

A single database will always encounter various limitations, so it is better to take the strengths and make use of external assistance.

Also, by slicing the database vertically or horizontally, we can break through the access limitations of a single database or data table and improve the performance of the query.
