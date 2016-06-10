# Class 3 - SQL & Some Python Stuff

+ What to expect
+ Student Introductions

## SQL

Don't forget Allison's notes are your best resource for detailed explanations of the things we've covered in class.

### Review of Aggregate Functions

+ `SELECT sum(population) FROM country;`  same as selecting a column and looking at the sum of it in Excel.
+ `SELECT max(area) FROM country;`  find the largest country in the world
+ `SELECT name FROM country WHERE area = 17075200;`  use the result from the last query to structure this one.

> `SELECT` actually does two things: **filtering** and **aggregation**. While these two tasks may be separated in other languages, SQL kind of conflates the two tasks and uses the `SELECT` command to handle both tasks.

+ `SELECT max(depth) FROM lake;` this is not really a selection from the old table -- it's like a new table based on the calculations from the old table. `SELECT` is used for aggregation, not filtering in this case.
    + Any time you see an aggregate function you're working with a NEW table that SQL is creating based on the results of your aggregation
    + If you try to pull out `lake.name`, the query will not work because the NEW table does not have a `name` column (though your initial table did).
+ Count is another aggregate function
    + `SELECT sea, count(*) from river group by sea;`
    + `SELECT sea, count(*) from river group by sea HAVING count(*) > 1;`

### Grouping Aggregates using `GROUP BY`

+ `GROUP BY` is written after the `FROM` clause of your select statement
+ **ONLY** works if you have some sort of aggregation function in your query
+ Sample: `SELECT max(population) from city WHERE country = USA;`
+ `SELECT country, max(population) FROM city GROUP BY country`
    + This is going to find the maximum population for each country it finds in the city table.
    + Whatever field you specify in the `GROUP BY` you can include in your `SELECT`.
+ `SELECT DISTINCT(type) FROM island;`: only return unique values from the 'type' column from the 'island' table.

**IMPORTANT POINT:** If there's an aggregate in your `SELECT` clause, you're working with a new, temporary table that no longer contains the columns of your original table!

#### Nuances of `WHERE` + `HAVING`

+ `SELECT country, max(population) FROM city GROUP BY country WHERE max(population) > 8000000;`
    + This will not work; aggregate function cannot be included in a WHERE clause.
    + `WHERE` is specifically used in the first, filtering step of `SELECT`
+ `SELECT country, max(population) FROM city WHERE elevation > 2000 GROUP BY country;`
    + The `WHERE` applies to the first part of the `SELECT` clause; this works!
+ `SELECT max(population) FROM city WHERE elevation > 2000;`
    + **VISUAL OF HOW THIS RUNS:** city table --> `WHERE` --> `MAX(...)` --> NEW TABLE

+ There's another keyword/clause called `HAVING`
    + Same as `WHERE` class
    + Operates on new aggregate table, rather than the original table

### JOIN

Purpose of `JOIN ... ON ...` : connecting data from two (or more) separate tables

**Sample Structure:**

+ `SELECT ___ FROM city JOIN country ON city.country = country.code WHERE ...`
+ `SELECT city.name, country.name, city.population FROM city JOIN country ON city.country = country.code WHERE city.population > 55000 AND city.population 60000;`
    + you use `JOIN ... ON` based on columns shared across the two tables you're trying to join
+ `SELECT * FROM city JOIN country ON city.country = country.code WHERE city.population > 55000 AND city.population 60000;`
    + In English: for each value in this country field, find the record which matches the 'code' column in the country table and just tack that data onto the city table.

## Common Errors/Tips & Tricks

+ Exiting SQL query errors using `'` and `;`
    + If you see these database characters next to your database name in **psql** it means that you may have missed a `'` or `;`
    + Run those characters to exit the error
+ **TIP:** If your query has an error, sometimes reordering a clause will fix the error or give you a more descriptive/helpful error.


### IPython Notebook/Jupyter

[Installation Notes](https://github.com/ledeprogram/data-and-databases/#week-2-may-31-and-june-2)
