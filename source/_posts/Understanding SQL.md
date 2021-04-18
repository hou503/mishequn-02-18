---
title: Understanding SQL
date: 2020-09-13 22:40:39
tags: SQL
category: SQL
---
In our daily work, we use database management systems like MySQL and Oracle, which actually follow the SQL language, which means that we deal with these databases through the SQL language. So for people who are in the programming or Internet industry, the most powerful language in the middle is SQL. Ever since SQL joined the TIOBE programming language rankings, it has remained in the top 10.

Arguably the most important and versatile meta-foundation in the entire digital world is data, and the language that works directly with data is SQL. Many people overlook the importance of the SQL language, thinking that it's just the SELECT statement and that mastering it should be the business of the data analyst. In fact, you shouldn't underestimate the role of SQL in practice. Many business processes on the Internet today cannot be done without SQL, because they all deal with data.

What about the ubiquity of SQL in all kinds of technologies and businesses, and its ubiquity 45 years ago, in 1974, when IBM researchers published a paper unveiling database technology, SEQUEL: A Structured English Query Language, a structured query language that hasn't changed much to this day, with SQL's half-life compared to other languages! That's a very long time to say the least.

SQL has two important standards, SQL92 and SQL99, which represent the SQL standards that were released in '92 and '99 respectively, and the SQL language we use today still follows these standards. You know that '92 was when Windows 3.1 was released, and how many people still remember it today, but if you do data analysis, or work with data, you still use the SQL language.

As people in the technology and internet industries, we are always looking for a language that is versatile, relatively few changes, and relatively easy to learn, and SQL is one of the few languages that meets all three criteria.

SQL is so powerful, so will it be hard to learn? Not at all.SQL doesn't require as much programming language base to learn as other languages do!

The SQL language can be divided into the following four parts by function.

1. DDL, the Data Definition Language, is used to define our database objects, including databases, tables and columns. By using DDL, we can create, delete and modify databases and table structures.

2. DML, Data Manipulation Language, which we use to manipulate database related records, such as adding, deleting, and modifying records in data tables.

3. DCL, Data Control Language, which we use to define access rights and security levels.

4. DQL, the Data Query Language, which we use to query for the desired records, is the most important aspect of the SQL language. In real business, we deal with queries most of the time, so learning how to write correct and efficient queries is a key learning point.

SQL is one of the few declarative languages, and the thing about this language is that you just need to tell the computer what kind of results you want from the raw data. For example, if I want to find heroes whose main character position is a warrior, as well as their hero name and maximum lifespan, I can enter the following language.

`SELECT name, hp_max FROM heros WHERE role_main = 'hero'`

Here I've defined the heros table, which includes the fields name, hp_max, role_main, and so on.

As you can see from this code, I'm not telling the computer what to do to get the result, which is one of the greatest conveniences of declarative languages. We don't need to specify specific execution steps, such as which step to execute first, which step to execute second, whether to check that condition A is met before executing, and other such conventional programming thinking.

Isn't it great that the SQL language defines our requirements and a different DBMS extracts the desired results for us according to the specified SQL!

SQL is the language in which we communicate with the DBMS and we need to design the DBMS before we can create it, which in the case of the RDBMS is in the form of an ER diagram, or entity-relationship diagram.

After the ER diagram review is approved, we create the data table with SQL statements or visual management tools.

What is the use of Entity - Relationship Diagram? It is the conceptual model we use to describe the real world, and in this model there are 3 elements: entities, attributes, and relationships.

Entities are the objects we want to manage, attributes are the properties that identify each entity, and relationships are the relationships between objects.

Once we've created the table, we're ready to operate with SQL. You can see that a lot of SQL statements are not uniformly case sensitive, and while the case does not affect the execution of SQL, I would recommend that you use a uniform writing convention, as good code conventions are the key to efficiency.

On the subject of SQL case-by-case, I have summarized the following two points.

1. Table names, table aliases, field names, field aliases, etc. are all lowercase.
2. SQL Reserved words, function names, bind variables, etc. are all capitalized.

For example, the following SQL statement.

`SELECT name, hp_max FROM heros WHERE role_main = 'hero'`

You can see that SELECT, FROM and WHERE are all uppercase for common SQL reservations, while name, hp_max and role_main are all lowercase for table names. It is also recommended to use underscores for the field names in the table, such as role_main.

## Sum up
Today I've taken you through a preliminary understanding of the SQL language, but of course, no matter how simple SQL is, you still need to do it step by step, starting from a little bit, first master the basic DDL, DML, DCL and DQL syntax, then understand the differences in the SQL syntax of different DBMS, and then look at how to optimize and improve the efficiency of SQL. To write high performance SQL, you first need to understand how it works, followed by a lot of practice.

