---
title: "06 Aggregations"
teaching: 10
exercises: 10
questions:
- "How can I summarise the data in my tables"
- "How can I make sure my column names make sense?"
objectives:
- "Apply aggregate functions (MIN, MAX, AVG) to group records together"
- "Use the ‘group by’ clause to summarise data"
- "Use built-in statistical functions to provide column summaries"
- "Use the ‘having’ clause to provide selection criteria to the summary values"
- "Understand the difference between the ‘where’ and the ‘having’ clauses"
- "Saving a query to make a new table"
keypoints:
- "Builtin functions can be used to produce a variety of summary statistics"
- "The `DISTINCT` keyword can be used to find the unique set of values in a column or columns"
- "Data in columns can be summarised by values using the `GROUP BY` clause"
- "Summarised data can be filtered using the `HAVING` clause"
---

## Using built-in statistical functions

Aggregation allows us to combine results by greouping records based on certain values such as an average across a group of rows.

Let's go the Farms table and find out how many farms there are.   Using the wildcard * and the COUNT function to count the number of records (rows):
~~~ 
SELECT COUNT (*)
FROM Farms; 
~~~ 
{: .sql}

We can also find out the minimum, maximum and average number of years farmed using the 'A11_years_farm', by writing a query such as this;

~~~ 
SELECT 
       MIN(A11_years_farm),
       MAX(A11_years_farm),
       AVG(A11_years_farm)
FROM Farms; 
~~~ 
{: .sql}

This sort of query provides us with a general view of the values for a particular column or field across the whole table.
  
`MIN` , `MAX` and `AVG` are builtin aggregate functions in SQLite (and any other SQL database system). There are many other aggregate functions included in SQL, for example SUM .

> ## Challenge
>
> Write a query that returns: XXXXXXXXXX
>
> > ## Solution
> > ~~~
> > -- All animals
> > SELECT SUM(weight), AVG(weight), MIN(weight), MAX(weight)
> > FROM surveys;
> >
> > -- Only weights between 5 and 10
> > SELECT SUM(weight), AVG(weight), MIN(weight), MAX(weight)
> > FROM surveys
> > WHERE (weight > 5) AND (weight < 10);
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}



## The `group by` clause to summarise data

Now lets COUNT **how many** farms are in each different ward (in the 'A08_ward' column). 
We do this using the `GROUP BY` clause.

~~~ 
SELECT A08_ward, COUNT(*) AS How_many
FROM Farms
GROUP BY A08_ward;
~~~ 
{: .sql}

`GROUP BY` tells SQL what field or fields we want to use to aggregate the data.
If we want to group by multiple fields, we give `GROUP BY` a comma separated list. 

For example if we want to count how many farms there are in each province, district, ward and village we can write the following SQL command. 

The grouping will take place based on the order of the columns listed in the `GROUP BY` clause. There will be one row returned for each unique combination of the columns mentioned in the `GROUP BY` clause
~~~ 
SELECT A06_province, 
       A07_district,
       A08_ward,
       A09_village,
       COUNT(*) AS How_many
FROM Farms
GROUP BY A06_province, A07_district, A08_ward, A09_village
;
~~~ 
{: .sql}

> ## Exercise
> Write a query which returns the min, max and avg number of years farmed as well as a count of the number of records involved
> for the 'A11_years_farm' column for each village in the 'Nhamatanda' district.   
>
>
> > ## Solution
> >
> > ~~~
> >SELECT A09_village,
> >        MIN(A11_years_farm) AS min,
> >        MAX(A11_years_farm) AS max,
> >        AVG(A11_years_farm) AS avg,
> >        COUNT(*) AS how_many
> > FROM Farms
> > WHERE A07_district = 'Nhamatanda'
> > GROUP BY A09_village;
> > ~~~
> > 
> > Notice that you can use the 'A07_district' column in the `where` clause but it doesn't have to appear in the `select` clause.
> > 
> {: .solution}
{: .challenge}


## The `having` keyword 

In the previous episode, we used the keyword `WHERE`, allowing us to filter the results according to some criteria. 

SQL offers a mechanism to filter the results based on **aggregate functions**, through the `HAVING` keyword.
In a `HAVING` clause you can use the column alias to refer to the aggregated column.

For example, we can write a query to return the min, max and count of farms in each ward and to only return information about wards with more than 2 farms. 

~~~ 
SELECT A08_ward,
       MIN(A11_years_farm) AS min_years,
       MAX(A11_years_farm) AS max_years,
       COUNT(*) As how_many_farms
FROM Farms
GROUP BY A08_ward
HAVING how_many_farms > 2;
~~~ 
{: .sql}

You can use the `AS` keyword to assign an alias to a column or table, and refer to that alias in the `HAVING` clause.
For example, in the above query, we can call the `COUNT(*)` by another name, like `how_many_farms`. 


> ## Exercise
>
> Using the Crops table write a query which will list all of the crops (D_curr_crop) which are grown in over 100 plots.
> 
> > ## Solution
> > 
> > ~~~
> > SELECT D_curr_crop, COUNT(*) AS how_many
> > FROM Crops
> > GROUP BY D_curr_crop
> > HAVING how_many > 100
> > ;
> > ~~~
> > {: .sql}
> >
> {: .solution}
{: .challenge}