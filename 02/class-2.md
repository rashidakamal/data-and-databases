# Class 2 - SQL (cont'd)

Open up **Postgres** and **psql** and load up the mondial database! (We created the database during last class and imported our data).

### Quick review of commands from class 1

+ `create database [database_name]`: creates a database
+ `\l` : lists all the databases you have
+ `\c [database_name]`: connects to your database
+  `\i [file path]`: imports data; do first for the schema file and then for the inputs file

Once you've connected to your mondial database, use the command `\d` to view the tables contained in your database.

+ `\d`: view tables in your database
+ `\d [table_name]`: view the columns (and the data types of the stuff in the columns) contained in a specific table.

### Data types you may encounter

+ **varchar(n) or character varying(n)**: a string of length n
+ **integer:**: exactly what it sounds like
+ **float**: floating point numbers
+ **numeric:** fractional number with a fixed precision; floats take up less space but are less accurate than numerics. Numerics uses more space, but are always accurate
    + this has something to do with how your computer stores different data types
    + never store money values in floats!
+ **date:** day, month, year
+ **timestamp:** day, month, year, time (hour, minute, second)
+ **boolean:** true or false

## How to query databases!

### SQL query structure:

`SELECT [fields] FROM [table_name] WHERE [criterion] ORDER BY [expression, usually the name of a field] LIMIT [some number];`

+ The first component( `SELECT [fields] FROM [table_name]`) is called a **"select statement"**
+ `WHERE`, `ORDER BY`, and `LIMIT` are option components of a SQL query called **clauses**
+ Sample:
    + `SELECT name, population FROM city WHERE population > 7000000;`
    + `ORDER BY`: in order to order your results in descending order (the default is ascending), add `DESC` to your `ORDER BY` clause. Sample: `ORDER BY population DESC`

> **Remember:** You can use command line keyboard shortcuts to move around in psql as well! Remember using &darr; and &uarr; will allow you to move through previously entered commands. A few other helpful ones for Mac:
>    + Ctrl+A: move to the beginning of a line in command line
>    + Ctrl+E: move to the end of the line
>    + Ctrl+U: to delete a whole line
> You'll probably have to "Use Option as Meta key" under Settings, Keyboard Preferences
>    + Option + b/left: move back one word
>    + Option + f/right: move forward one word

Note that you can write your SQL queries across multiple lines -- just be sure to end it with a `;`.

We went through several sample queries in class that added additional clauses to the query above -- check [Allison's notes](https://github.com/ledeprogram/data-and-databases/blob/master/SQL_notes.md#writing-a-query-with-select) to review!

### Using `*`

Using an `*` in your select statement allows you to grab all the columns of that table (you don't have to specify like we did above). The query will look something like this:

````
SELECT * FROM sea LIMIT 10;`
````

**Another trick:** `SELECT count(*) FROM [table_name];` will give you the number of rows in your table.

### Adding conditions using WHERE

Just like in Python where we can ask our if-statements to print something only if certain conditions are met, we can use `WHERE` in SQL to specify which rows of our table we want. Basically, we're saying "I want the rows where X condition is true."

Some of the operators we can use to specify our conditions are:

+ `>` : greater than or equal to
+ `<` : less than or equal to
+ `>=` : greater than or equal to
+ `<=` : less than or equal to
+ `=` : exactly equal to
+ `!=` : not equal to
+ `<>` : not equal to

This list is no where near exhaustive! Lots of other operators out there.

> **Note:** if you are passing a string in your query, you must use **SINGLE QUOTES, NOT DOUBLE QUOTES**. Example: `SELECT * FROM city WHERE name = 'New York City';`

### More complex conditions using `AND` & `OR`

Sample:

`SELECT * FROM lake WHERE elevation > 4000 AND depth > 400;`

+ `AND`: returns results where both results are true
+ `OR`: returns results where either or both results are true (`OR` is **not exclusive**.)
+ You can use parentheses to make these much more complex.

> Use `Ctrl+C` to abort a process or return to the input line

This will not work:

`SELECT * FROM lake WHERE country = 'E' OR 'F';`

We have to write:

`SELECT * FROM lake WHERE country = 'E' OR country = 'F';`

Using particular field names in the query does not necessitate that it must be shown in the output.

### Aggregate functions

We saw one earlier! `Select count(*) from lake;`.

`SELECT count(*) FROM city WHERE country = 'F';`

**Other aggregate functions**:
+ avg([field_name])

`SELECT avg(population) FROM country WHERE area < 10000;`

+ sum([field_name])

`SELECT sum(population) FROM city WHERE country='SF';`

+ max([field_name])

`SELECT max(population) from country;`

+ min([field_name])

## Common errors

+ You forgot to enter `;` at the end of your SQL query (Check whether there is a `=` or `-` by your database name. A minus sign means that you've forgotten to enter a semi-colon.)
+ You have a loose `'` or `"` out there (in this case, there will be a single or double quote by your database name; enter a quote of the displayed type and then enter a semi-colon to pop out of the error).
