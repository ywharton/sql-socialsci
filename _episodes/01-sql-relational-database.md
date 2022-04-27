---
title: "01 What is a relational database?"
teaching: 10
exercises: 5
questions:
- "What is a relational database and why should I use it?"
- "Why do tables have key columns?"
- "What is SQL"

objectives:
- "Define a relational database"
- "Compare with other types of databases"

keypoints:
- "A relational database is data organised as a collection of related tables"
- "SQL (Structured Query Language) is used to extract data from the tables. Either a single table or data spread across two or more related tables."

---

## What is a relational database?

A relational database is a collection of data items organised as a set of tables. Relationships can be defined between the data in one table and the data in another or many other tables.
The relational database system will provide mechanisms by which you can query the data in the tables, re-assemble the data in various ways without altering the data in the actual tables. This querying is usually done using SQL (Structured Query Language). SQL allows queries to be constructed from the use of only a few keywords.
Databases are designed to allow efficient querying against very large tables, more than the 1M rows allowed in an Excel spreadsheet.

## What is a table?

As were have noted above, a single table is very much like a spreadsheet. It has rows and it has columns. A row represents a single observation and the columns represents the various variables contained within that observation. 
Often one or more columns in a row will be designated as a 'primary key'. This column or combination of columns can be used to uniquely identify a specific row in the table. 
The columns typically have a name associated with them indicating the variable name. A column always represents the same variable for each row contained in the table. Because of this the data in each column will always be of the same *type*, such as an Integer or Text, of values for all of the rows in the table. Datatypes are discussed in the next section.


## Why do tables have primary key columns?

Whenever you create a table, you will have the option of designating one of the columns as the primary key column. The main property of the primary key column is that the values contained in it must uniquely identify that particular row. That is you cannot have duplicate primary keys. This can be an advantage which adding rows to the table as you will not be allowed to add the same row (or a row with the same primary key) twice.

The primary key column for a table is usually of type Integer although you could have Text. For example if you had a table of car information, then the "Reg_No" column could be made the primary key as it can be used to uniquely identify a particular row in the table.

A table doesn't have to have a primary key although they are recommended for larger tables. A primary key can also be made up of more than one column, although this is less usual.


## What different types of keys are there?

In addition to the primary key, a table may have one or more _Foreign keys_. A foreign key does not have to be unique or identified as a foreign key when the table is created. A foreign key in one table will relate to the primary key in another table. This allows a relationship to be created between the two tables. If a table needs to be related to several other tables, then there will be a foreign key  (column) for each of those tables.

## How does the database represent missing data?

All relational database systems have the concept of a NULL value. NULL can be thought of as being of all data types or of no data type at all. It represents something which is simply _not known_.

When you create a database table, for each column you are allowed to indicate whether or not it can contain the NULL value. Like primary keys, this can be used as a form of data validation.

In many real life situations you will have to accept that the data isn't perfect and will have to allow for NULL or missing values in your table.

In DB Browser we can indicate how we want NULL values to be displayed. We will use a RED background to the cell to make it stand out. In SQL queries you can specifically test for NULL values.

We will look at missing data in more detail in a later episode.

