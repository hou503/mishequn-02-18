---
title: DBMS
date: 2020-09-20 21:20:35
tags: DBMS
category: SQL
---
## What is the difference between DB, DBS and DBMS?

When it comes to DBMS, there are a few concepts you need to understand.

A DBMS, database management system, can actually manage multiple databases, so you can understand that DBMS = multiple databases + management program.

A database is a collection of stored data and you can think of it as multiple data tables.

Database system. It's a much larger concept that includes databases, database management systems, and DBAs, database managers.

It's important to note here that although we sometimes call Oracle, MySQL, etc. databases, they should be called database management systems, or DBMS, to be precise.

A relational database is a database based on a relational model, and SQL is the query language for relational databases.

Compared to SQL, NoSQL refers to non-relational databases in general, including key-value databases, document databases, search engines and column stores on the list, as well as graph databases.

Key-Value databases store data by means of Key-Value keys, where Key and Value can be simple or complex objects. key as a unique identifier, the advantage is that it is faster to find, which is significantly better than a relational database, but also has the obvious disadvantage that it can not use conditional filtering as freely as a relational database. If you don't know where to look for data, you have to traverse all the keys, which consumes a lot of computation. A typical use case for key-value databases is as a content cache. redis is the most popular key-value database.

Document databases are used to manage documents, which are the basic unit of information processed in a database.

Search engines are also an important application in database retrieval, and common full-text search engines include Elasticsearch, Splunk and Solr. although relational databases use indexes to improve retrieval efficiency, full-text indexes are less efficient.

Columnar database is relative to the row-type storage database, Oracle, MySQL, SQL Server and other databases are using row-type storage, and columnar database is to store data to the database in accordance with the column, the advantage of doing so is that you can greatly reduce the system's I/O, suitable for distributed file system, the shortage is relatively limited in function.

Graph databases, which make use of a data structure such as a graph to store relationships between entities. The best example is the relationship between people in a social network, the data model is implemented mainly in nodes and edges and is characterized by the ability to solve complex relationship problems efficiently.

There are many categories of NoSQL, such as key-value, document, search engine, columnar storage, and graphical databases, all of which fall into the NoSQL camp. It is only by using the term NoSQL that these technologies can be encompassed. Even so, the SQL camp is much heavier in the DBMS rankings, with 4 of the top 5 most influential DBMSs being relational databases and 12 of the top 20 DBMSs being relational databases. So, mastering SQL is very necessary.

This is because SQL has been dominating the DBMS, so many people wondered if there was a database technology that could move away from SQL, so NoSQL was born, but as it evolved, it became more and more indispensable, and the DBMSs in the NoSQL camp all have SQL-like features.

NoSQL is a great complement to SQL, and it allows us to make better use of database technologies, such as fast reads and writes, that can be scaled more easily and inexpensively in the age of cloud computing.

Twelve of the top 20 DBMSs are in the SQL camp, with the top three DBMSs being in the SQL camp: Oracle, MySQL, and SQL Server.

In 1979, Oracle 2 was born, the first commercially available RDBMS, which was later sold to military customers. As the fame of Oracle software grew, the company changed its name to Oracle Corporation. in the 1990s, Oracle founder Ellison became the second richest man after Bill Gates, and it can be said that IBM created two empires, one with Microsoft, the dominant software industry, and the other with Oracle, the dominant enterprise software market. today, Oracle's annual revenue is $40 billion, which is a testament to the value of commercial database software. Oracle's annual revenue has reached $40 billion, which is enough to prove the value of commercial database software. We can also see from this point, if you choose a big track, it is necessary to commercialize as soon as possible to occupy large enterprise customers can create tremendous business value, but also enough to prove that a software enterprise does not need to rely on selling hardware can also make a lot of money.

MySQL is an open source database management system that was born in 1995, and because of its free and open source features, it was loved by developers and its user base grew rapidly, making it the No.1 open source database. Oracle bought it, and Oracle also had the management rights of MySQL, so Oracle became the absolute leader in the database field. From here we can also see that although MySQL is a free product, but the number of users is enough to prove the great user value. A product with great user value, even if it has no direct business value, will be valued by business giants as infrastructure.

However, at the same time that Oracle acquired MySQL, the creators of MySQL were concerned about the risk of MySQL being closed source, so they created a MySQL offshoot project, MariaDB, which is for the most part compatible with MySQL and adds many new features, such as support for more storage engines type. Many businesses have also switched from MySQL to MariaDB.

SQL Server is a commercial database developed by Microsoft and was born in 1989. In fact, Microsoft also introduced the Access database, which is a desktop database, with both background storage and front-end interface development functions, more lightweight, suitable for small-scale applications. Because the background storage space is limited, generally only 2G, Access has the advantage of being able to easily develop the interface in the foreground. SQL Server, on the other hand, is a large database, used for background storage and querying, and does not have the functions of interface development. For example, Oracle is more suitable for large multinational enterprises because they are not cost-sensitive, but have higher requirements for performance and security, while MySQL is more popular with many Internet companies, especially early-stage entrepreneurs. Company favorites.

## Sum up
In 1974, the SEQUEL paper was published, in 1979 the first commercial relational database Oracle 2 was created, in 1995 the MySQL open source database was created, and today, NoSQL has evolved and the DBMS race around the SQL standard has never stopped. In this history, there are both SQL camp, and NoSQL camp, both commercial database software, and open source products, in different application scenarios, the same company may also have different DBMS layout.

If different DBMSs represent the interests of different companies, then as users we should pay more attention to the scenarios of these DBMSs. For example, Oracle, as the commercial database software with the highest market share, is suitable for large multinational enterprises, while for lightweight desktop databases, we can use Access. For free and open source products, we can use MySQL or MariaDB, and in the NoSQL camp, we also need to understand the difference between key-value, document, search engine, columnar database and graph database.

