---
title: The data table paradigm
date: 2020-10-25 17:32:59
tags: The data table paradigm
category: Database
---
## What are the design paradigms for the database?
When designing a relational database model, the need to define the degree of rationalization of the connections between the various attributes within the relationship gives rise to different levels of specification requirements, which are called paradigms. You can think of a paradigm as a level of design criteria that the design structure of a data table needs to meet.

There are currently six paradigms for relational databases, which are, in order of paradigm level, from lowest to highest: 1NF, 2NF, 3NF, BCNF (Bass-Cord paradigm), 4NF (fourth paradigm), and 5NF (fifth paradigm, also known as the perfect paradigm).

The higher the paradigm design of the database, the lower the redundancy, and at the same time the higher-order paradigm must meet the requirements of the lower-order paradigm, for example, to meet 2NF must meet 1NF, to meet 3NF must meet 2NF, and so on.

In general data tables should be designed to meet 3NF as much as possible. but not absolutely. sometimes it is necessary to break the paradigm rules to improve the performance of certain queries. i.e., anti-normalization.

## Those keys in the data table
The definition of a paradigm will use primary and candidate keys, and the keys in the database will consist of one or more attributes. I summarize the following definitions of keys and attributes that are commonly used in data tables.

The set of attributes that uniquely identifies a tuple is called a hyperkey.
Candidate key: if the super key does not include any extra attributes, then this super key is a candidate key.
Primary key: the user can select one of the candidate keys as the primary key.
Foreign key: If an attribute set in the data table R1 is not the primary key of R1, but the primary key of another data table R2, then this attribute set is a foreign key of the data table R1.
Primary attribute: the attribute included in any candidate key is called primary attribute.
Non-primary attributes: as opposed to primary attributes, attributes that are not included in any of the candidate keys.

From 1NF to 3NF
Now that you know the four types of keys in a database table, let's look at 1NF, 2NF, and 3NF, which we'll talk about later.

1NF refers to the fact that any attribute in a database table is atomic in nature and cannot be subdivided. In fact, any DBMS will satisfy the first paradigm by not splitting fields.

2NF refers to a data table in which non-primary attributes have a full dependency on the candidate key of the table. A full dependency is not the same as a partial dependency, i.e., you cannot depend on only part of a candidate key's attributes, but you must depend on all of them!

This means that a field in the candidate key determines the non-primary attribute. You can also understand that for non-primary attributes, it is not entirely dependent on the candidate key. What kind of problem does this cause?

Data redundancy: If a player can play in m matches, then the player's name and age are duplicated m-1 times. A match may also have n players in it, the time and place of the match is duplicated n-1 times.
Insert Exception: If we want to add a new match, but we haven't decided who the players are yet, then it can't be inserted.
Delete exception: if we want to delete a player number, the match information will be deleted at the same time if we don't have a separate match sheet.
Update Exception: If we adjust the time of a match, then all the times in the table for that match need to be adjusted, otherwise the time of one match will be different from the other.

## Sum up

Relational databases are designed based on a relational model, in which there are four types of keys, and the central role of these keys is identification.

Building on these concepts, I talk about 1NF, 2NF, and 3NF, three paradigms that we often work with to build databases that are less redundant and more structured.

One thing to keep in mind is that these paradigms only present design criteria, and in practice, the design of data tables may not have to conform to these principles. On the one hand, there are problems with these paradigms, which can lead to anomalies in insertions, updates, deletions, etc. (these will be illustrated in the next section), and on the other hand, they can reduce the efficiency of queries. Why is this? Because the higher the paradigm level, the more data tables are designed, multiple tables may need to be associated when querying data, which affects the efficiency of the query.

