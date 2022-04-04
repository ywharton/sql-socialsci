---
title: "Accessing Data with Queries"
teaching: 30
exercises: 5
questions:
- "What is SQL?"
- "How can I write basic queries in SQL?"

objectives:
- "Define  SQL" 
- "Create simple SQL queries to return rows and columns from existing tables"
- "Filter data given various criteria"
- "Sort the results of a query"

keypoints:
- "Strictly speaking SQL is  a standard, not a particular implementation"
- "SQL implementation are sufficiently close that you only have to learn SQL once"
- "The SELECT statement allows you to 'slice' and 'dice' the columns and rows of the dataset so that the query only returns the data of interest"
---

## Definition of SQL 
SQL stands for Structured Query Language. SQL allows us to interact swith data in a relational database through queries. These queries allow us to perform a number of actions such as inserting, selecting, updating, and deleting information in a database. 

In SQL, querying data is performed by a SELECT statement. A select statement has 6 key components;

~~~ 
SELECT colnames
FROM tablename
WHERE conditions
GROUP BY colnames
HAVING conditions
ORDER BY colnames
~~~ 
{: .sql}

## Writing your first SQL query - the Select statement
 
Let's start by using the **farms** table. Here we have data on every individual farm. 

Let’s write an SQL query that selects all of the columns in the farms table. SQL queries can be written in the box located under the "Execute SQL" tab. 
Click on the right arrow above the query box to execute the query. (You can also use the keyboard shortcut "Cmd-Enter" on a Mac or "Ctrl-Enter" on a Windows machine to execute a query.) 
The results are displayed in the box below your query. 
If you want to display all of the columns in a table, use the wildcard *.

~~~ 
SELECT *
FROM Farms;
~~~ 
{: .sql}

We have capitalized the words SELECT and FROM because they are SQL keywords. SQL is case insensitive, but it helps for readability, and is good style.
The '*' character acts as a wildcard meaning all of the columns but you cannot use it as a general wildcard.
So for example, the following is not valid.
~~~ 
SELECT A*
FROM Farms;
~~~ 
{: .sql}

If you run it you will get an error.
When an error does occur you will see an error message displayed in the bottom pane. 

If we want to select a single column, we can type the column name instead of the wildcard *.
~~~ 
SELECT Country
FROM Farms;
~~~ 
{: .sql}

If we want more information, we can add more columns to the list of fields (such as A06_province, A07_district, A08_ward, A09_village),right after SELECT:

~~~ 
SELECT Country, A06_province, A07_district, A08_ward, A09_village
FROM Farms;
~~~ 
{: .sql}


### Limiting results

Sometimes you don't want to see all the results, you just want to get a sense of what's being returned. In that case, you can use the `LIMIT` command. In particular, you would want to do this if you were working with large databases. In the example below only the first ten rows of the result of the query will be returned.  This is useful if you just want to get a feel for what the data looks like. 

~~~ 
SELECT *
FROM Farms
LIMIT 10;
~~~ 
{: .sql}

> ## Exercise
>
> Write a query which returns the first 5 rows from the Farms table with only the columns Id, and B16 to B20.
> 
> > ## Solution
> >  ~~~
> > SELECT  Id
> >       , B16_years_liv
> >     , B17_parents_liv
> >     , B18_sp_parents_liv
> >     , B19_grand_liv
> >     , B20_sp_grand_liv
> > FROM Farms
> > LIMIT 5;
> > ~~~
> > {: .sql}
> > Because the query uses several columns (with longish names), for readability they have been set out on separate lines. SQL takes 
> > of white space to you are free to arrange the text of the query as you like.  
> {: .solution}
{: .challenge}

### Unique values

If we want only the unique values so that we can quickly see what farms have been sampled we can use `DISTINCT` 
Using the farms table we can obtain a list of all the different vlaues of the 'A06_province' column contained in the table.

~~~
    SELECT DISTINCT A06_province
    FROM surveys;
~~~
> > {: .sql}

If we select more than one column, then the distinct pairs of values are
returned

    SELECT DISTINCT year, species_id
    FROM surveys;

## Filtering - using the `Where` clause

Databases can also filter data – selecting only the data meeting certain criteria i.e. certain values or ranges within one or more columns. 
For example, let’s say we only want data for the in rows where the value in the B16_years_liv column is greater than 25.   

We need to add a `WHERE` clause to our query:


In this example we are only interested in rows where the value in the B16_years_liv column is greater than 25

~~~
SELECT  Id, B16_years_liv
FROM Farms
WHERE B16_years_liv > 25
;
~~~ 
{: .sql}

In addition to using the '>' we can use many other operators such as <, <=, =, >=, <> 
~~~
SELECT  Id, B17_parents_liv
FROM Farms
WHERE B17_parents_liv = 'yes'
;
~~~ 
{: .sql}

### Using more AND and OR logical expressions

We can also use more sophisticated criteria by combining tests with AND and OR keywords.
For example, suppose we want the data where both sets of parents live in the house and both have grandchildren:

~~~
SELECT  Id
FROM Farms
WHERE (B17_parents_liv = 'yes') AND (B18_sp_parents_liv = 'yes') AND (B19_grand_liv = 'yes') AND (B20_sp_grand_liv = 'yes') 
;
~~~ 
{: .sql}

Notice that the columns being used in the `WHERE` clause do not need to returned as part of the `SELECT` clause. 
You can ensure the precedence of the operators by using brackets and also aid readability

~~~
SELECT  Id
FROM Farms
WHERE (B17_parents_liv = 'yes' OR B18_sp_parents_liv = 'yes') AND B16_years_liv > 60
;
~~~ 
{: .sql}

> ## Exercise
>
> From the above query, breakdown the `Where` clause so that each component can be tested individually.
> Make a note of how many rows are returned in each case.
> 
> > ## Solution
> > 
> > To test each of the `or` clauses
> > 
> > ~~~
> > SELECT  Id
> > FROM Farms
> > WHERE B17_parents_liv = 'yes'
> > ;
> > SELECT  Id
> > FROM Farms
> > WHERE B18_sp_parents_liv = 'yes'
> > ;
> > SELECT  Id
> > FROM Farms
> > WHERE (B17_parents_liv = 'yes' OR B18_sp_parents_liv = 'yes') 
> > ;
> > SELECT  Id
> > FROM Farms
> > WHERE B16_years_liv > 60
> > ;
> > ~~~
> > {: .sql}
> > 
> > `OR` generally creates a less restrictive condition and `AND` makes a more restrictive condition.
> > 
> {: .solution}
{: .challenge}

### Building more complex queries
Now let's combine qeuries to get data from the farms table where the value of B16_years_liv is in the range 51 to 59 inclusive. 
We could use an AND operator 

~~~
SELECT Id, B16_years_liv
FROM Farms
WHERE B16_years_liv > 50 AND B16_years_liv < 60
;
~~~ 
{: .sql}


The same results could be obtained by using the BETWEEN or IN operators

~~~
SELECT Id, B16_years_liv
FROM Farms
WHERE B16_years_liv BETWEEN 51 AND 59
;
~~~ 
{: .sql}


~~~
SELECT Id, B16_years_liv
FROM Farms
WHERE B16_years_liv IN (51, 52, 53, 54, 55, 56, 57, 58, 59)
;
~~~ 
{: .sql}

The list of values in brackets do not have to be contiguous or even in order.

> ## Exercise
>
> Write a query using the Farms table which returns the columns Id, A09_village, A11_years_farm, B16_years_liv. We are only interested in rows where the A09_village value is either 'God' or 'Ruaca'. Additionally
> we only want A11_years_farm values in the range 20 to 30 exclusive and B16_years_liv values strictly greater than 40.
> There are many ways of doing this, but try to use an inequality, an `IN` clause and a `BETWEEN` clause.
> 
> > ## Solution
> > 
> > ~~~
> > SELECT Id, A09_village, A11_years_farm, B16_years_liv
> > FROM Farms
> > WHERE     A09_village IN ('God', 'Ruaca') 
> >       AND A11_years_farm BETWEEN 21 AND 29 
> >       AND B16_years_liv > 40
> > ;
> > ~~~
> > 
> {: .solution}
{: .challenge}



## Sorting results 

We can also sort the results of our queries by using `ORDER BY`.
For simplicity, let’s go back to the **farms** table and select the rows where the village is 'God'
and alphabetize it by years farmed.

~~~
SELECT Id, A09_village, A11_years_farm, B16_years_liv
FROM Farms
WHERE A09_village = 'God';
~~~ 
{: .sql}

Now let's order it by years on the farm (A11_years_farm).

~~~
SELECT Id, A09_village, A11_years_farm, B16_years_liv
FROM Farms
WHERE A09_village = 'God'
ORDER BY A11_years_farm;
~~~ 
{: .sql}

By default the SQL assumes Ascending order. You can make this more explicit by using the `ASC` or `DESC` keywords.

~~~
SELECT Id, A09_village, A11_years_farm, B16_years_liv
FROM Farms
WHERE A09_village = 'God'
ORDER BY A11_years_farm DESC
;
~~~ 
{: .sql}

You can also order by multiple columns

~~~
SELECT Id, A09_village, A11_years_farm, B16_years_liv
FROM Farms
WHERE A09_village = 'God'
ORDER BY A11_years_farm DESC , B16_years_liv ASC
;
~~~ 
{: .sql}
 
