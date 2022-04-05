---
title: "Saving queries for future use - creating views"
teaching: 20
exercises: 10
questions:
- "How can I create a view in DB Browser for SQLite using SQL code?"
- "How can I add records of data to a table?"
objectives:
- "Create views using SQL code"

keypoints:
- "A View can be treated just like a table in a query"
- "A View does not contain data like a table does, only the instructions on how to get the data"
---


## Using SQL code to create views

It is not uncommon to repeat the same operation more than once, for example for monitoring or reporting purposes. SQL comes with a very powerful mechanism
to do this by creating views. Views are a form of query that is saved in the database, and can be used to look at, filter, and even update information. One way to
think of views is a table, that can read, aggregate, and filter information from several places before showing it to you.

For example, imagine that we'd like to see the location information about our farms (Country, Province, District, Ward and Village) .  That
query would look like:

~~~
SELECT Id, 
       Country, 
       A06_province, 
       A07_district,
       A08_ward, 
       A09_village
FROM Farms;
~~~
{: .sql}

But we don't want to have to type that every time we want to ask a question about that particular subset of data. Hence, we can benefit from a view.
Creating a view from a query requires us to add `CREATE VIEW viewname AS` before the query itself. 

~~~
CREATE VIEW vFarms_location AS 
SELECT Id, 
       Country, 
       A06_province, 
       A07_district,
       A08_ward, 
       A09_village
FROM Farms;
~~~
{: .sql}

It is common practice when creating Views to indicate somewhere in the name that it is in fact a View. e.g. vFarms_location or Farms_location_v.

Using a view we will be able to access these results with a much shorter notation:
~~~
SELECT *
FROM vFarms_Location;
~~~
{: .sql}


Although tables and views can be used almost interchangeably in `Select` queries it is important to note that a `View` unlike a `Table` contains no data. 
The advantage of using Views is that it allows you to restrict how you see the data in a table. 
In the example we used above it may be far easier to work with only the 6 columns that we need from the full Farms table 
rather than the full table with 61 columns.

A View isn't restricted to simple `Select` statements it can be the result of aggregations and joins as well. 
This can help reduce the complexity of queries based on the View and so aid readability. 
