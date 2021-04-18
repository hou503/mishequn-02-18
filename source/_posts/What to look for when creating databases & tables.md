---
title: What to look for when creating databases & tables
date: 2020-10-10 22:49:02
tags: SQL
category: SQL
---
DDL is a core component of DBMS and an important part of SQL, and the correctness and stability of DDL is an important foundation for the entire SQL operation. Different developers may create very different databases and tables for the same requirement, so what are the good principles when designing a database?

## Basic syntax and design tools of the DDL

In DDL, we often use the commands CREATE, DROP and ALTER to add, delete and modify, respectively.

1. Definition of the database

```sql
CREATE DATABASE nba; 
DROP DATABASE nba; 
```

2. Definition of data tables

```sql
CREATE TABLE table_name
```

### Creating a table structure

For example, we want to create a player table with the name player and two fields, one is player_id, which is of type int, and the other is player_name, which is of type varchar(255). Neither of these fields is empty, and player_id is incremental.

Then when created it could be written as.

```sql
CREATE TABLE player  (
  player_id int(11) NOT NULL AUTO_INCREMENT,
  player_name varchar(255) NOT NULL
);
```
Note that statements are terminated by a semicolon (;) and that there is no comma at the end of the definition of the last field. Data type int(11) represents an integer type with a display length of 11 bits, and the parameter 11 in parentheses represents the maximum valid display length, regardless of the size of the range of values contained in the type. varchar(255) represents a variable string type with a maximum length of 255. not NULL indicates that the entire field cannot be empty, and is a data Constraints.AUTO_INCREMENT stands for automatic growth of the primary key.

In fact, we seldom write DDL statements ourselves and can use some visual tools to create and manipulate databases and tables. I would like to recommend Navicat, which is a database management and design tool that is cross-platform and supports many kinds of database management software, such as MySQL, Oracle, MariaDB, etc. In this case, I would like to recommend Navicat, which is a database management and design tool that is cross-platform and supports many kinds of database management software. 

So a player table is designed. The code is as follows.

```sql
DROP TABLE IF EXISTS `player`;
CREATE TABLE `player`  (
  `player_id` int(11) NOT NULL AUTO_INCREMENT,
  `team_id` int(11) NOT NULL,
  `player_name` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `height` float(3, 2) NULL DEFAULT 0.00,
  PRIMARY KEY (`player_id`) USING BTREE,
  UNIQUE INDEX `player_name`(`player_name`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

```
You can see the DDL processing throughout the SQL file, first deleting the player table first, then creating the player table, which uses inverted quotes for both the data table and the field names, this is to avoid them having the same names as the MySQL reserved fields.

The player_name field has utf8 as its character set, and the sorting rule is utf8_general_ci, which means that it is not case sensitive, and if it is set to utf8_bin, it means that it is case sensitive, and many other sorting rules are not described here.

Since player_id is set as the primary key, it is specified in the DDL using PRIMARY KEY, and indexed using BTREE.

The difference between UNIQUE INDEX and normal index is that it uses Uniqueness Constraints. You can choose between BTREE or HASH for the indexing method, which is used here. I will explain the difference between BTREE and HASH indexing method later.

The entire data table is stored using InnoDB, which is the default storage engine after MySQL version 5.5, as we have mentioned before. Also, by setting the character set to utf8, the sorting rule to utf8_general_ci, and the row format to Dynamic, we can define the final convention for the data table as follows.

```sql
ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```

### Modifying the table structure
After creating the table structure, we can also modify the table structure, although using Navicat directly will ensure that the re-exported table is up-to-date, it is necessary to understand how to use the DDL command to modify the table structure.

1. Add a field, e.g. I add an age field of type int(11) to the data table

```sql
ALTER TABLE player ADD (age int(11));
```

2. Modify the field name to change the age field to player_age

```sql
ALTER TABLE player RENAME COLUMN age to player_age
```

3. Change the data type of the field, set the data type of player_age to float(3,1).
```sql
ALTER TABLE player MODIFY (player_age float(3,1));
```

4. Delete the field, delete the player_age field you just added.
```sql
ALTER TABLE player DROP COLUMN player_age;
```


### Common constraints on data tables
When we create a data table, we also impose constraints on the fields, which are designed to ensure the accuracy and consistency of the data within the RDBMS. Let's take a look at what the common constraints are.

The first is the primary key constraint.

The primary key is used to uniquely identify a record and cannot be duplicated or null, i.e. UNIQUE+NOT NULL. A primary key can be a single field, or a composite of several fields. In the example above, we set player_id as the primary key.

This is followed by the foreign key constraint.

Foreign keys ensure the integrity of table-to-table references. A foreign key in one table corresponds to the primary key of another table. A foreign key can be duplicate or null. For example, player_id is the primary key in the player table. If you want to set a player score table, i.e. player_score, you can set player_id as a foreign key in player_score, which is associated with the player table.

In addition to constraints on keys, there are also field constraints.

Uniqueness constraint.

A uniqueness constraint indicates that a field has a unique value in the table, even if we already have a primary key, but we can also impose uniqueness constraints on other fields. For example, if we set a uniqueness constraint on player_name in the player table, it indicates that no two players can have the same name. It should be noted that there is a difference between a uniqueness constraint and a NORMAL INDEX. A uniqueness constraint creates a constraint and a normal index to ensure that the fields are correct, while a normal index only speeds up data retrieval and does not constrain the uniqueness of the fields.

NOT NULL Constraint. Defines NOT NULL for a field, which indicates that the field should not be empty and must have a fetch value.

DEFAULT, which indicates a default value for the field. If this field does not have a value when the data is inserted, it is set to the default value. For example, we set the height field to DEFAULT 0.00 by default.

CHECK constraint, used to check the validity of the value range of a specific field, the result of the CHECK constraint can not be FALSE, for example, we can CHECK the height of the value, must be ≥ 0, and <3, that is, CHECK(height>=0 AND height<3).

### Principles for designing data tables
When we design data tables, we often consider various questions, such as: what data do users need? What data needs to be stored in a data table? What data is frequently accessed? How can you improve the efficiency of your search?

How do you ensure the correctness of data in a data table, and what kind of constraint checking should be done when inserting, deleting, or updating?

How do you reduce data redundancy in data tables to ensure that they don't expand rapidly as the number of users grows?

How can we make the database more accessible to those responsible for its maintenance?

In addition to this, the scenarios in which we use databases vary, and it is fair to say that data tables may be designed very differently for different situations.

1. The smaller the number of data tables, the better.

The core of the RDBMS is the definition of entities and links, i.e., the E-R diagram. The fewer data tables there are, the more concise the design of the entities and links proves to be, both easy to understand and easy to operate.

2. The smaller the number of fields in the data table, the better.

The greater the number of fields, the greater the potential for data redundancy. The premise of setting the number of fields low is that the fields are independent of each other, rather than that the value of one field can be calculated by the other fields. Of course, having a small number of fields is relative, and we usually balance data redundancy with search efficiency.

3. The fewer the number of fields in a data table that have a joint primary key, the better.

The primary key is set to determine uniqueness, and when a field cannot be uniquely determined, a joint primary key is used (i.e., multiple fields are used to define a primary key). The more fields in the joint primary key, the more index space it takes up, which not only makes it more difficult to understand, but also increases the running time and index space, so the fewer fields in the joint primary key, the better.

4. The more primary keys and foreign keys you use, the better.

The design of a database is really about defining various tables, and the relationships between various fields. The more of these relationships there are, the less redundancy and higher utilization these entities prove to be. This has the advantage of not only ensuring the independence of the data tables from each other, but also increasing the utilization of the correlations between them.

## Sum up
Today we learned the basic syntax of DDL, such as how to define a database and database tables.

When creating a data table, apart from defining the field names and data types, the most common constraints we consider are the constraints on the fields.

