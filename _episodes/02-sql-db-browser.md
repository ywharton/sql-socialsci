---
title: "Using DB Browser for SQLite"
teaching: 30
exercises: 0
questions:
- "What does the DB Browser for SQLite allow me to do?"
objectives:
- "Understand the layout of the DB Browser for SQLite and the key facilities that it provides"
- "Connect to databases"
- "Create new databases and tables"

keypoints:
- "The DB Browser for SQLite application allows you to connect to an existing database or create a new database"

---

## Setup

We use DB Browser for SQLite and the SQL SAFI database/ dataset throughout this lesson. See the setup instructions on how to download the data, and also how to install the DB Browser for SQLite.

## Motivation
To start let's orient ourselves in our project workflow. Previously we used Excel and OpenRefine to go from messy, human created data to cleaned, computer-readable data.  Now we're going to move to the next piece of the data workflow, using the computer to read in our data, so we can then access it for analysis and visualisation.

## Dataset description
The data we will be using is a subset of data collected from the SAFI (Studying African Farmer-Led Irrigation) project.  This project  is looking at farming and irrigation methods. This is survey data relating to households and agriculture in Tanzania and Mozambique. The survey data was collected through interviews conducted between November 2016 and June 2017 using forms downloaded to Android Smartphones.

The survey covered such things as; household features (e.g. construction materials used, number of household members), agricultural practices (e.g. water usage), assets (e.g. number and types of livestock) and details about the household members.


## Launching DB Browser

In Windows the installation of DB Browser does not create a desktop icon. To explicitly launch the application after installing it, use the windows button (bottom left of screen) and type in ‘DB Browser’ in the search bar and selecting the application when it appears.

![DB Browser run](../fig/DB_Browser_install_2.png)

## The Initial screen

The initial screen of DB Browser will look something like this, the panes may be in a different configuration;

![DB Browser initial screen](../fig/DB_Browser_run_1.png)

There is;

A small menu system consisting of File, Edit, View and Help.
Below the menu system is a toolbar with four options; New Database, Open Database, Write Changes and Revert Changes.
Below the toolbar is a 4-tabbed pane for; Database Structure, Browse Data, Edit Pragmas and Execute SQL. Initially these will be quite empty as we haven't created or opened a database yet. In general we will see how each of these are used as we go through the lesson with the exception of the Edit Pragmas tab which deals with system wide parameters which we won't want to change.

On the right hand side there are two further panes, at the top is the Edit Database Cell pane which is grayed out. Below it is a 3-tabbed pane for DB Schema, SQL log and Remote. We are only really interested in the DB Schema tab. 

## Initial changes to the layout.
 
The overall layout of DB Browser is quite flexible. The panes on the right-hand side can be dragged and dropped into any position, the individual tabs on the bottom pane closed directly from the pane and re-opened from the menu View item.

We will make a couple of initial changes to the layout of the screen. These will be retained across sessions.

1. From the View menu item un-select the 'Edit Database Cell' icon to the left of the text. This will make the pane close and the bottom pane will be expanded automatically to fill the space.
2. a) On Windows, From the View menu item select 'preferences' and select the Data Browser tab.
2. b) On Mac, From the "DB Browser for SQLite" menu item select 'preferences' and select the Data Browser tab.

![Data Browser Preferences](../fig/DB_Browser_run_2.png)

Towards the bottom there is a section dealing with Field colors. You will see three bars below the word Text, to the right there are in fact three invisible bars for the Background. Click in the area for the Background color for NULL. A colour selector window will open, select Red. The bar will turn Red. This is now the default background cell colour that will be used to display NULL values in you tables. We will discuss the meaning of NULL values in a table in a later episode.

 You can now close the preference window by clicking OK.

## Opening a database

Let's look at a pre-existing database, the  [SQL_SAFI.sqlite](../data/SQL_SAFI.sqlite) file from the SAFI project that you downloaded during the setup. 

To open the database in DB Browser do the following;
1. Click on the 'Open Database' button in the toolbar.
2. Navigate to where you have stored the database file on your local machine, select it and click 'Open'.

You can see the table sin the database by looking at the left hand side of the screen under the 'Database Structure' tab. Here you will see a list under 'Tables'.  To see the contents of any table , click on it, and then click the 'Browse Data' next to the 'Database Structure'.  This will give us a view that we are used to - a copy of the table.  Hopefully this helps to show that a database is, in some sense, just a collection of tables, where there's some value in the tables that allows them to be connected to each other (the 'related' part of the 'relational database'). Also note there are options for 'New Record' and 'Delete Record'. As our interest is in analysing existing data not creating or deleting data, it is unlikely that you will want to use these options. 

The 'Database Structure' tab also provides some metadata about each table. If you click on the down arrow next to a table name, you will see information about the columns, which in databases are referred to as 'fields', and their assigned data types (the rows of a database table are called *records*). Each field contains one variety or type of data, often numbers or text. You can see in the XXX table that most fields contain numbers (BIGINT, or big integer, and FLOAT, or floating point numbers/decimals) while the XXX table is entirely made up of text fields.

The "Execute SQL" tab is blank now - this is where we'll be typing our queries to retrieve information from the database tables.


![Table Actions](../fig/DB_Browser_run_3.png)

To summarize:

* Relational databases store data in tables with fields (columns) and records (rows)
* Data in tables has types, and all values in a field have the same type ([list of data types](#datatypes))
* Queries let us look up data or make calculations based on columns

## Database Design

* Every row-column combination contains a single *atomic* value, i.e., not containing parts we might want to work with separately.
* One field per type of information
* No redundant information
    * Split into separate tables with one table per class of information
    * Needs an identifier in common between tables – shared column - to reconnect (known as a *foreign key*).


## Import

Before we get started with writing our own queries, we'll create our own database.  We'll be creating this database from the three `csv` files we downloaded earlier.  Close the currently open database (**File > Close Database**) and then follow these instructions:

1. Start a New Database
    - Click the **New Database** button
    - Give a name and click Save to create the database in the opened folder
    - In the "Edit table definition" window that pops up, click cancel as we will be importing tables, not creating them from scratch
2. Select **File >> Import >> Table from CSV file...**
3. Choose `farms.csv` from the data folder we downloaded and click **Open**.
4. Give the table a name that matches the file name (`farms`), or use the default
5. If the first row has column headings, be sure to check the box next to "Column names in first line".
6. Be sure the field separator and quotation options are correct. If you're not sure which options are correct, test some of the options until the preview at the bottom of the window looks right.
7. Press **OK**, you should subsequently get a message that the table was imported.
9. Back on the Database Structure tab, you should now see the table listed. Right click on the table name and choose **Modify Table**, or click on the **Modify Table** button just under the tabs and above the table list.
10. Click **Save** if asked to save all pending changes.
11. In the center panel of the window that appears, set the data types for each field using the suggestions in the table below (this includes fields from the `plots` and `crops` tables also).
12. Finally, click **OK** one more time to confirm the operation. Then click the **Write Changes** button to save the database.

> ## Challenge
>
> - Import the `plots` and `crops` tables
{: .challenge}

You can also use this same approach to append new fields to an existing table.

## Adding fields to existing tables

1. Go to the "Database Structure" tab, right click on the table you'd like to add data to, and choose **Modify Table**, or click on the **Modify Table** just under the tabs and above the table.
2. Click the **Add Field** button to add a new field and assign it a data type.

## <a name="datatypes"></a> Data types

| Data type                          | Description                                                                                              |
|------------------------------------|:---------------------------------------------------------------------------------------------------------|
| CHARACTER(n)                       | Character string. Fixed-length n                                                                         |
| VARCHAR(n) or CHARACTER VARYING(n) | Character string. Variable length. Maximum length n                                                      |
| BINARY(n)                          | Binary string. Fixed-length n                                                                            |
| BOOLEAN                            | Stores TRUE or FALSE values                                                                              |
| VARBINARY(n) or BINARY VARYING(n)  | Binary string. Variable length. Maximum length n                                                         |
| INTEGER(p)                         | Integer numerical (no decimal).                                                                          |
| SMALLINT                           | Integer numerical (no decimal).                                                                          |
| INTEGER                            | Integer numerical (no decimal).                                                                          |
| BIGINT                             | Integer numerical (no decimal).                                                                          |
| DECIMAL(p,s)                       | Exact numerical, precision p, scale s.                                                                   |
| NUMERIC(p,s)                       | Exact numerical, precision p, scale s. (Same as DECIMAL)                                                 |
| FLOAT(p)                           | Approximate numerical, mantissa precision p. A floating number in base 10 exponential notation.          |
| REAL                               | Approximate numerical                                                                                    |
| FLOAT                              | Approximate numerical                                                                                    |
| DOUBLE PRECISION                   | Approximate numerical                                                                                    |
| DATE                               | Stores year, month, and day values                                                                       |
| TIME                               | Stores hour, minute, and second values                                                                   |
| TIMESTAMP                          | Stores year, month, day, hour, minute, and second values                                                 |
| INTERVAL                           | Composed of a number of integer fields, representing a period of time, depending on the type of interval |
| ARRAY                              | A set-length and ordered collection of elements                                                          |
| MULTISET                           | A variable-length and unordered collection of elements                                                   |
| XML                                | Stores XML data                                                                                          |


## <a name="datatypediffs"></a> SQL Data Type Quick Reference

Different databases offer different choices for the data type definition.

The following table shows some of the common names of data types between the various database platforms:

| Data type                                               | Access                    | SQLServer            | Oracle             | MySQL          | PostgreSQL    |
|:--------------------------------------------------------|:--------------------------|:---------------------|:-------------------|:---------------|:--------------|
| boolean                                                 | Yes/No                    | Bit                  | Byte               | N/A            | Boolean       |
| integer                                                 | Number (integer)          | Int                  | Number             | Int / Integer  | Int / Integer |
| float                                                   | Number (single)           | Float / Real         | Number             | Float          | Numeric       |
| currency                                                | Currency                  | Money                | N/A                | N/A            | Money         |
| string (fixed)                                          | N/A                       | Char                 | Char               | Char           | Char          |
| string (variable)                                       | Text (<256) / Memo (65k+) | Varchar              | Varchar2 | Varchar        | Varchar       |
| binary object	OLE Object Memo	Binary (fixed up to 8K)   | Varbinary (<8K)           | Image (<2GB)	Long | Raw	Blob          | Text	Binary | Varbinary     |

