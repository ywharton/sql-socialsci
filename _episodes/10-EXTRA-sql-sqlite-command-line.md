---
title: "10 EXTRA: The SQLite command line"
teaching: 15
exercises: 10
questions:
- "How can I save my code in a file and run it again?"
objectives:
- "Use the SQLite shell to re-run a file of SQL code"
- "Save the output from the SQLite shell to a file"

keypoints:
- "SQLite databases can be created, managed and queried from the SQLite shell utility"
- "You can run the shell interactively from the commandline, typing queries or dot cammands at the prompt"
- "You can call the SQLite3 program and specify a database and a set of commands to run. This aids automation"
---

## Running SQL code using the SQLite shell

Before you can run the SQLite3 shell program you must have installed it. Instructions for doing this are included in
 the [set up procedures](../setup.md).

I will assume that you have added the location of the program to your local PATH environment variable as this 
will make it easier to refer to the database file and other files we may want to use. 

The instructions in this episode are written from a Windows user perspective. If you are using Linux or a Mac, 
open a terminal window instead a command prompt.

1. Open a command prompt (cmd.exe) and 'cd' to the folder location of the SQL_SAFI.sqlite database file.
2. run the command 'sqlite3' This should open the SQLite shell and present a screen similar to that below.

![SQLite shell](../fig/SQL_08_SQLite_shell.png)

3. By default a "transient in-memory database" is opened. You can change the database by use of the *.open* command

~~~
.open SQL_SAFI.sqlite
~~~
{: .bash}

It is imprtant to remember the .sqlite suffix, otherwise a new database simply called SQL_SAFI would be created

4. Once the database is opened you can run queries by typing directly in the shell. Unlike in DB Browser, 
you must always terminate your select command with a ";". This is how the shell knows that **You** think the statement is complete. Although easy to forget, it generally works to your advantage as it allows you to split a long query command across lines as you did in the DB Browser application.

![SQLite shell query example](../fig/SQL_08_SQLite_shell_query_example.png)

The output from the query is displayed on the screen. If we just wanted to look at a small selection of data this 
may be OK. It is however more likely that not only are the results from the query somewhat larger, 
but also we would prefer to save the output to a file for later use. There are some other changes to the output format that we might want to change as well.
for example; change the field seperator from the default "\|" to a comma and provide column headers. This will make the
output more like a standard csv file.

Notice that the `NULL` values in columns 6 and 8 are just left empty, two consequetive delimiters, just as they are in a csv file.


We can make the changes we want by using further "dot" commands.

There are in fact a large number of "dot" commands and they are all explained in the official SQLite documentation [here](https://sqlite.org/cli.html). One you will have to use at some point is `.quit` which will end the SQLIte shell program.

The commands we need are 

~~~
> .mode csv
~~~
{: .bash}

to change the field seperatator to ",". There are many other modes available see the [documentation](https://sqlite.org/cli.html). 

~~~
> .header on
~~~
{: .bash}

to show the column headers 

~~~
> .output my.filename
~~~
{: .bash}

to direct the output to a file of my choice. 
The file will be created if needed or it will overwrite an already existing file, so exercise care.

![SQLite shell dot commands](../fig/SQL_08_SQLite_shell_dot_commands.png)

Yes you can have a file called "my.filename" if you want. The contents of which contains the expected output from the query.

![SQLite my.filename](../fig/SQL_08_my_filename.png)

Notice the use of quotes in the rows where the value of the data item themselves contain quotes; in this case single quotes. 

## Automating the use of the SQLite shell

So far we have used the shell in much the same way as we might have used the DB Browser application. 
We run the program, connect to a database, run a query and save the output. 
Because the shell will accept any valid SQL statements as well as have numerous 'dot' commands of it own 
to configure how it works it could be considered as powerful as the DB Browser application. 
You could use it as a replacement in most cases. 

Most people prefer to work with nice point and click interfaces, so why would you want to use the shell rather than the DB Browser application?

The shell has one distinct advantage over DB Browser; you can run the shell program and in the call to the program provide a parameter indicating 
the database to connect to and provide a file of the commands that you want to execute. The shell will execute the file of commands and then exit.

Here is an example

1. create a file `commands.sql` containing the following content:

~~~
result.csv
.mode csv
.output results.csv
.open SQL_SAFI.sqlite
SELECT * from Farms where A09_village='God';
~~~
{: .sql}


2. run the sqlite3 program in the following way

~~~
$ sqlite3 < commands.sql
~~~
{: .sh}


Notice that there is no output to the screen and that the shell is closed. The results of running the query have been placed in the results.csv file.

There are two key advantages of using this approach.

1. It aids automation. It would be straightforward to have the one line commandline instruction to be run automatically, perhaps on a timed basis. The SQL statements in the executed file doesn't have to be a simple query. It could be appending rows of data to a series of tables which become available on a regular basis.

2. It aids reproducibility. Although it is convenient to use the DB Browser application to play around and try things out, eventually you will decide on approach, create relevant queries to perform your analysis or research and at this point you will need to ensure that the complete sequence is documented and is reproducible. This is what the file of SQLite commands will do for you.

> ## Exercise
>
> The query
> 
> ~~~
> SELECT * from Farms WHERE C01_respondent_roof_type = 'grass';
> ~~~
> {: .sql}
>
> returns all of the records from the Farms table which have a roof made of grass. 
>
> Create a file of SQL statements and SQLite shell commands to create 3 files each containing the output from queries like the above but for all three different
> roof types (grass, mabatisloping and mabatipitched)
>
> > ## Solution
> >  The contents of your file should be something like this:
> > 
> >~~~
> >.mode csv
> >.output grass_roofs.csv
> >select * from Farms where C01_respondent_roof_type = 'grass';
> >.output mabatisloping_roofs.csv
> >select * from Farms where C01_respondent_roof_type = 'mabatisloping';
> >.output mabatipitched_roofs.csv
> >select * from Farms where C01_respondent_roof_type = 'mabatipitched';
> >~~~
> > {: .output}
> >
> > The command to run it from the commandline is:
> > 
> > ~~~
> > sqlite3 SQL_SAFI.sqlite < SQLite_commands.sql
> > ~~~
> > {: .bash}
> > 
> {: .solution}
{: .challenge}
