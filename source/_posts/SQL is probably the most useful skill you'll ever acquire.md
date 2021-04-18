---
title: SQL is probably the most useful skill you'll ever acquire
date: 2020-09-05 20:22:37
tags: SQL
category: SQL
---
In 1946, the world's first computer was born, and today, the Internet, which was developed by this computer, has become a river of its own. In these decades, countless technologies and industries have been floating in this river, some are still emerging, and some have gone through several ups and downs.

But in the midst of all this volatility, there is one technology that never goes away, and that is SQL.

SQL, as a language that deals directly with data, is a language that interacts with a variety of front-end and back-end languages.

Both front-end engineers and back-end algorithm engineers are bound to work with data and need to understand how to extract the data they want quickly and accurately. Not to mention data analysts, whose job it is to work with data and collate different reports in order to guide business decisions.

Although technical people use SQL to a greater or lesser extent, different people write SQL with different levels of efficiency; for example, a good SQL execution plan minimizes I/O operations because I/O is the most bottlenecked area of the DBMS, and arguably a lot of time is spent on I/O in database operations.

In addition, you also need to consider how to reduce CPU computation; using GROUP BY, ORDER BY, etc. in SQL statements consumes a lot of CPU computation resources, so we need to take a global view and consider not only the I/O performance of the database, but also CPU computation, memory usage, and so on.

For example, an EXIST query and an IN query can get the same result in some cases, but which is more efficient when it comes to execution?

Suppose I abstract the pattern as follows.

`SELECT * FROM A WHERE cc IN (SELECT cc FROM B)`

`SELECT * FROM A WHERE EXIST (SELECT cc FROM B WHERE B.cc=A.cc)`

During the query process, we need to determine the size of Table A and Table B. If Table A is larger than Table B, then the IN subquery is more efficient than the EXIST subquery. If table A is larger than table B, then the IN subquery is more efficient than the EXIST subquery.

Of course, the power of SQL extends far beyond IT technology to the product and operations side.

For example, let's say you're the product manager of a game and you want to find out what heroes are available under various conditions, such as who are the mage heroes with a max life value greater than 7000. Did you ask R&D for help? Or are you looking slowly from the mass of data?

Of course it works both ways, but it would be embarrassing to call R&D every time, and it would be inefficient to do it yourself.

In fact with a single SQL statement, you can get the answer directly from the data table.

`SELECT * FROM heros WHERE hp_max >= 7000 AND role = 'heroes'`

SQL statements are so intuitive that you can figure out what they mean from a basic level of English, even if you don't have any SQL. That's the best thing about SQL.

What if you're an operations person and you want to see how many new users you've added in 7 days? First we need to get the current time, just use the NOW() function, then convert it to days, compare it with the user's registration time, less than 7 days is our filtering criteria, and finally we can get the desired data.

`SELECT COUNT(*) as num FROM new_user WHERE TO_DAYS(NOW())-TO_DAYS(regist_time)<=7`

The two examples above are relatively simple SQL queries, but SQL can also help you keep track of daily new, daily active, and next-day retained data.

In fact, besides business, SQL is also used in various data-based technologies, such as OLTP (online transaction processing), OLAP (online analytical processing), RDBMS (object-based relational database management system). Even on the NoSQL front, SQL-like operations are being used today, bearing in mind that the concept of NoSQL was originally conceived as a move away from SQL, but today people prefer to define NoSQL as Not Only SQL. In addition, in our familiar data formats such as XML, JSON, etc., there are various kinds of SQL, such as SQL for XML, SQL for JSON, etc. In addition, there are also SQL for XML, JSON, etc., and there are also SQL for JSON. In addition, there are also SQL for geolocation information, SQL for search, SQL for time series data, SQL for streams, and so on.

It can be said that SQL is needed for both business and data-related technologies.

If you're programming or in the Internet industry, there's nothing more valuable than learning the language SQL, and SQL is probably the most useful skill you'll ever acquire. The need to understand data tends to be high-frequency, so mastering SQL yourself is essential in the real world.

Whether you're a product manager, an operations person, a developer, or a data analyst, you can use the SQL language. It's like a sword that not only increases your productivity, but also expands your work horizons.

1. Basic chapter
The syntax of SQL is very simple, just like English, but it is very powerful and helps us to index, sort, group, etc. on data. However, these commands are used differently in different database management systems, so in this column, I will not only focus on the syntax of SQL itself, but also explain how this syntax is used in different database management systems like MySQL, Oracle, SQL Server, and so on.

2. Advanced
Many people who write SQL are faced with the question, "I'm also querying data with SQL, why do I write statements slower than others?"

In fact, it's the simplicity of SQL syntax that causes many people to write in an unconventional way, such as confusing the order of keywords, which inadvertently reduces the efficiency of SQL execution.

In this section, I'll explain the problems you often encounter when using SQL in practice, and how to use tools to analyze and quickly locate performance problems and solutions. 3.

3. Advanced section
In the era of big data, there are many database management systems for different scenarios, including SQL-based databases, such as Oracle, MySQL, SQL Server, Access, WebSQL, SQLite, etc., and NoSQL-based databases, such as MongoDB, Redis, etc. In this section, I will talk about the use of various mainstream database management systems.

In this part, I will talk about the use of various mainstream database management systems.

4. practical chapter
The above sections are to help you organize your knowledge of SQL, but only when you learn how to use SQL systematically for real projects, can you really put what you have learned into practice and make SQL work for you.




