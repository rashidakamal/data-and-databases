# Course Intro & SQL Basics

+ About the class: [syllabus and notes](https://github.com/ledeprogram/data-and-databases)
+ [Allison is the coolestttt.](http://www.decontextualize.com/)

## Main takeaways from DDB

+ What decisions go into data and databases? How do these decisions reflect the biases of those who made them?
+ Data is not neutral; **data is forgetting**.
+ [About forgetting.](http://theanarchistlibrary.org/library/ursula-k-le-guin-a-non-euclidean-view-of-california-as-a-cold-place-to-be)
+ Reality is analog, unknowable -> through scanning, sampling, digitizing, transcribing -> we get data that is discrete, digital, "black and white"

### Thinking about data

+ Who produced the data?
+ For what purpose?
+ Why choose this phenomenon for digitization instead of some other phenomenon?
+ Whose labor produced the data?
+ What biases and assumptions are built into the data and the systems that store, process and use the data?

**Technical skills should be critical skills.** Why does this data or tool exist?

## SQL

+ Stands for **Structured Query Language**; used for getting information into and out of databases.
+ **How it works**: you write a query in SQL that you then send to a database, and then the database sends back the results of your query.
+ SQL is used with **relational databases**. (Other types of databases include hierarchal, flat DB, key/value, graphic, NoSQL, etc.)
    + A **relational database** is made up of tables.
    + Relational Database Management software (**RDBMS**) you may have heard of: MySQL, PostgreSQL, SQL Server, Oracle. (We use programs to store databases on computers. You can use SQL to communicate with these programs, though each one speaks a slightly different dialect of SQL.)

**Why use text as a medium for computer programming (versus using a more familiar graphical interface)?** It's precise & repeatable!

If you want to check out a few sample SQL queries, Allison has some great examples in her notes under ['A quick taste'](https://github.com/ledeprogram/data-and-databases/blob/master/SQL_notes.md#a-quick-taste).

### Clients & Servers

+ When we use SQL, there are basically two computers involved: your computer (which has a tiny client program) and another computer on the internet somewhere (which has the server).
+ Usually, your computer does not have the database software -- client software lets you to write queries that allow you to access/communicate with the server (which sends you back results). Data storage is on the server.
+ Server is sometimes used to mean the software on the computer, or the computer running it.
+ This is called **client-server architecture**. It's how the internet works! Very different having Microsoft Office software on your computer.

**NOTE:** when we use Postgres in class, our computers will act as both the client and the server! psql is our client and Postgres is our server. Theoretically, if the server was on another computer, you could have any number of clients accessing the server.

### Using psql

**psql** is our tiny client software for connecting with Postgres. psql will open a Terminal window (on windows, your user name will be "postgres" and on Mac, it'll be your username).

+ `help` to learn more
+ `\q` to quit and return to your UNIX prompt on OS (this only quits from the client part) -- On OS: you have choose 'Quit' under the Postgres elephant icon on your menubar (top right). Windows conflates the two (seemingly).

There are two different kinds of commands: SQL commands and psql commands. psql commands only controls your tiny client software (psql); SQL commands are actually sent to the server! All of the psql commands begin with a backslash `\ `.

How to import data into a server:
+ We'll use a preexisting dataset.
+ We need to tell our client software to run the SQL commands we downloaded from [MONDIAL](http://www.dbis.informatik.uni-goettingen.de/Mondial/).
    + We downloaded two files: one containing the database schema and the other containing input statements.

As per usual, don't type the `[ ]` in the commands below -- those are just there to show where you may enter other names/characters.

+ `create database [mondial]` : we're creating and naming our database 'mondial'
+ `drop database [mondial]` : remove the mondial database (**don't do this** because we want to keep using our mondial database)
+ `\l` : shows you all the databases you have on your server (you should see mondial there)
+ `\c [mondial]` : connect to the mondial database
+ `\i [PATH]` : import data into your database; use this command for both the schema file and the inputs file. So: `\i [file path to schema file]` and then `\i [file path to inputs file]`
    + you can drag and drop to get the file paths

Woohoo, you have your database -- we'll learn how to query our database next time!
