# Class 3 - Movies Database & SQL in Python

+ HW2 preview
+ IPython Notebooks
+ Many-to-many joins
+ movielens (csv -> SQL)
+ pg8000


## Homework Preview

+ You can find the HW assignments on [Github](https://github.com/ledeprogram/data-and-databases)
+ Click on the assignment, click "Raw" --> File, Save Page As --> save to the appropriate directory
+ In Terminal/Babun, use `cd` to navigate to the folder you've saved your file.
+ If your downloaded file has a .txt extension rather than .ipynb, you will need to rename your file: `mv [old name of file] [new name of file]`
+ Run `jupyter notebook` in Terminal/Babun
+ You'll only modify the parts of the cells where it specifies "your query here"

## IPython/Jupyter Notebooks

**Why write in Python and not just do all this in Postgres?** You'll eventually want to run scripts that retrieve data from a database, manipulate it, and possibly add new data into your database.

When you launch ipython notebook (using `jupyter notebook` in Terminal/Babun), on that first "home" screen displaying your current working directory, you can create new ipython notebook.  

+ You can then export it as html
+ Or save as ipython notebook and upload onto Github (Github knows how to parse it and display it properly)
+ When you are "clicked into" a cell, you can hit **ESC** to be in command mode
    + There are lots of keyboard shortcuts to use in command mode
    + **Cmd + H:** shows keyboard shortcuts
+ Click into a cell or hit enter in a cell to exit command mode/enter edit mode (a pencil appears in the upper right corner). This is where you will enter your python code!
    + Run the cell by hitting **Shift + Enter/Return**; gets you new cell below it
    + **Ctrl + Enter:** runs cell; no new cell
    + **In[ 1 ]:** This shows your the order in which your cells have been executed.
    + **In [ * ]:** This means that the cell is still running
    + Use the onscreen up and down arrows to to move around your code
    + Usually, you will want to run your cells in the order that they're display (but it is technically possible to run the cells in a different order)

TWO different kinds of cells: **code cells** and **markdown cells**.
+ Cell --> Type --> Markdown
    + If you are in a cell, you can also change the type using the available dropdown menu
+ If you're in Command mode, you can just hit M.
+ Use markdown cells to describe what is happening in your code, observations, etc.
+ Great way to show people your process, reproduces your result AND describes your result

Fluency will come from practice!

## Back to SQL/psql

[Allison's notes](https://github.com/ledeprogram/data-and-databases/blob/master/SQL_notes.md#joining-with-many-to-many-relationships)

Connect to your mondial database in psql. (`\c mondial`).

### Linking Tables

Based on Allison's example of reporters and their articles, let's consider the situation in which a particular article has TWO authors. We could list both authors in the author column, OR we could list each article for each author.

**OUR SOLUTION:** One table has all the authors and another table has all the articles. Each author has a unique identifier and each article has a unique identifier. We have another thing called the **linking table** -- it only contains author ids and article ids. Only purpose of this table is to connect the two tables.
+ Linking tables aren't really keeping track of objects. There is no one *thing* each row refers to.
+ Each row is a record of an instance of a relationship between these two entities.
+ Linking tables allow us to represent the relationship between authors and articles without storing redundant data.
    + One of the purposes of relational databases is to reduce the number of places data is stored (Don't Repeat Yourself).
+ **Naming convention for linking tables:** "author_article".

### Many-to-Many Joins

Sooo, let's take a look back at our mondial database.

+ A **many-to-many relationship:** a river can go through many countries and a country can contain may rivers.
+ geo_ tables in our mondial databases are our linking tables
+ The following queries allow us to glimpse at the linking table between country and rivers (well, technically provinces and rivers):
    + `select * from geo_river where river = 'Amazonas'`
    + `select * from geo_river where country = 'SF'`

Other kinds of relationships:

+ **Many-to-one:** cities to a country
+ **One-to-one:** people and social security numbers (which SHOUlD be true..)
+ Representing **many-to-many relationships** efficiently is sort of the hallmark of **relational databases**!

#### Sample queries about rivers

+ `SELECT * FROM geo_river WHERE river = 'Rhein'`: this gives us back country codes, rather than country names
    + We have to do a `JOIN` in order to get the country names
    + `SELECT geo_river.river, country.name, geo_river.province FROM geo_river JOIN country ON geo_river.country = country.code WHERE geo_river.river = 'Rhein';`
    + `SELECT DISTINCT(country.name) FROM geo_river JOIN country ON geo_river.country = country.code WHERE geo_river.river = 'Rhein';`: all unique values for that country; names of the countries in which the Rhein river flows.

#### Sample queries about lakes

Let's look at the lake table. `\d lake` -- we may suspect many-to-many relationship `\d geo_lake`

+ Let's focus on big lakes: `SELECT name from lake where area > 50000;`
+ Every relationship between a country's province and a lake, where the area of the lake is greater than 5000: `SELECT lake.name, geo_lake.country, lake.area FROM geo_lake JOIN lake ON geo_lake.lake = lake.name WHERE lake.area > 50000;`
+ You can tack on as many JOINs as you want to based on some matching value between those tables.
    + `SELECT lake.name, country.name, lake.area, geo_lake.province FROM geo_lake JOIN lake ON lake.name = geo_lake.lake JOIN country ON country.code = geo_lake.country WHERE lake.area > 50000;`
+ This is what's powerful about relational databases!

**Student Question:**
+ Which country has the most lakes? `SELECT country, COUNT(country) from geo_lake GROUP BY country ORDER BY COUNT(country) DESC;`
    + This might just be giving all records, our geo_lake table lists one lake multiple times because it's really linking lakes to provinces, rather than USA.
+ Check: there are 35 total records rivers in a country... `SELECT count(*) FROM geo_river WHERE country = 'USA'`
    + So it is actually giving us back the number of provinces, not lakes!
+ Now we get what we want: `SELECT country, COUNT(DISTINCT lake) from geo_lake GROUP BY country ORDER BY COUNT(DISTINCT lake) DESC;`
+ **AGGREGATE + JOIN on a many-to-many table:** `SELECT country.name, COUNT(DISTINCT lake) from geo_lake JOIN country ON geo_lake.country = country.code GROUP BY country.name ORDER BY COUNT(DISTINCT lake) DESC LIMIT 1;`

> **NOTE** Allison often starts writing her queries with FROM and then goes back to SELECT. This may help you think through how you structure your queries.

## MovieLens Data

+ Grouplens, University of Minnesota
+ Data will come to you in many forms!
    + data separated by pipes (|)
    + data separated by tabs
    + data separated by commas
+ README: this is where I explain myself, how to interpret the data
+ What does the MovieLens data look like?
+ u.data is a linking table! user to ratings, ratings to movies.

**How to import csv Movielens data into Postgres:**

+ Can't do this until we make the table exist! `\copy [table_name] from [path to file on hard drive] delimiter [, or | or \t] csv`
+ `CREATE DATABASE movielens`
+ `\c movielens`
+ `\d` (no relations found, we haven't done anything yet)
+ `CREATE TABLE [table name] (
    fieldname type,
    fieldname type,
    fieldname type,
    )`: this command
    + Only other command we've learned is SELECT
    + Data types: varchar(n), int
+ We want to **create a schema** for this table.
````
CREATE TABLE uuser (
    user_id int,
    age int
    gender varchar(1),
    occupation varchar(30),
    zip_code varchar(5)
    )
````
+ `\d uuser`: see what it looks like
+ `select * from uuser;`: nothing in our table yet
+ **FYI:** `user` is a reserved word in SQL
+ **Import data from the csv into our new tables:** `\copy uuser from [file path to your downloaded data file] delimiter '|' csv`
+ Now this will work: `SELECT * from uuser LIMIT 10;`

**Note:** This is a different process than what we used to load the mondial database -- in that case, someone had already "setup" the database for us, written the schema, etc.

**[Allison's notes](https://github.com/ledeprogram/data-and-databases/blob/master/CSV_to_SQL.ipynb) on importing data from csvs to SQL is an excellent resource if you want more detail than what we went over in class.**

> **Note for Windows users:** For the homework, you are asked to import your data differently. If you run into an encoding error at that point, try passing `\encoding UTF8` and then use the import command `\i [filepath]`
