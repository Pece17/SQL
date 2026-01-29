# SQL

A project for learning the basics of **Structured Query Language**. My purpose is to learn to use **SQL** in a way that is applicable to real-life projects and work.


## Table of Contents

- [Software Installation and Setup](https://github.com/Pece17/SQL/blob/main/README.md#software-installation-and-setup)
- [Creating a Movie Rating Database](https://github.com/Pece17/SQL?tab=readme-ov-file#creating-a-movie-rating-database)


## Software Installation and Setup

First I need to install the prerequisites so I can get this project up and running. I just bought a second-hand **Lenovo Legion T5** PC that is running **Microsoft Windows 11 Home**, so performance shouldn't be an issue.

I'm going to use **PostgreSQL** because it is highly recommended and applicable to real-world jobs. I go to https://www.postgresql.org/download/windows/, select **Download the installer**, and download the latest (**18.1**) version for **Windows x86-64**. I run the installation file as administrator. I use the default setups and set a password when prompted **for the database superuser**, leave the default **Port** number, select **fi-FI** as the **Locale** (tells your computer or program how to handle language-specific stuff), and start the installation. I'm going to be using **ChatGPT** as a tutor during this project and also other learning resources, like [W3Schools](https://www.w3schools.com/sql/).

After the installation is finished, I run **SQL Shell (psql)** program and press **Enter** for all the following prompts to use the default choices:

```
Server [localhost]:
Database [postgres]:
Port [5432]:
Username [postgres]:
```

After this I'm asked to enter the password I chose during the installation:

```
Server [localhost]:
Database [postgres]:
Port [5432]:
Username [postgres]:
Password for user postgres:
```

After entering my password I get the following output:

```
Server [localhost]:
Database [postgres]:
Port [5432]:
Username [postgres]:
Password for user postgres:

psql (18.1)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

postgres=#
```

Now I'm succesfully connected to **PostgreSQL 18.1**. The warning message just means that this terminal uses a different character set than my computer, which supports e.g. Finnish characters. This probably won't be an issue as long as I don't use special characters in the database.

Now I can test if the database works correctly. The ```SELECT``` statement is used to select data from a database, so I enter the following **statement** to check the database version:

```
postgres=# SELECT version();
                                 version
-------------------------------------------------------------------------
 PostgreSQL 18.1 on x86_64-windows, compiled by msvc-19.44.35221, 64-bit
(1 row)
```

Next let's create a new database called ```testdb```, so that we avoid working in the default ```postgres``` database. The ```CREATE DATABASE``` **statement** is used to create a new **SQL** database. **Semicolons** (**;**) are also used after **statements** in **SQL**:

```
postgres=# CREATE DATABASE testdb;
CREATE DATABASE
postgres=#
```

Now we can connect to the newly created database using the ```\c``` **psql meta-command**:

```
postgres=# \c testdb
You are now connected to database "testdb" as user "postgres".
testdb=#
```

Let's create our first **table** in the ```testdb``` **database** with the ```CREATE TABLE``` **statement**:

- ```cars``` is the name of the table.
- ```id``` is the **column** name for the unique identifier for each car, ```SERIAL``` automatically generates numbers for new rows (**PostgreSQL**-specific and automatically creates a sequence behind the scenes), and ```PRIMARY KEY``` ensures this column is unique and cannot be empty (**NULL**).
- ```brand``` is the **column** name for the brand of the car (**Volvo**, **Toyota**, etc.), ```TEXT``` is the data type for any length of **string**/**text**, and ```NOT NULL``` means this field must have a value and you cannot insert a **row** without a brand.
- ```model``` is the **column** name for the model of the car (**V70**, **Celica**, etc.), and ```TEXT``` and ```NOT NULL``` work similarly as in the previous **row**.
- ```year``` is the **column** name for the manufacturing year of the car and ```INT``` is the data type for **integers** (whole numbers).
- ```created_at``` is the **column** name for when the row was created, ```TIMESTAMP``` is the data type for date and time, ```DEFAULT``` means default value to use if no value is provided during insertion, and ```now()``` is a **function** that returns the current date and time.
- Each **column** definition (not **row**) is separated by a **comma** ```,```.
- The **column** definitions are indented inside **parentheses** ```()```.

I use **Notepad++** to create the **statement** because it's easier than trying to write straight to **SQL Shell (psql)**. 

```
CREATE TABLE cars (
    id SERIAL PRIMARY KEY,
    brand TEXT NOT NULL,
    model TEXT NOT NULL,
    year INT,
    created_at TIMESTAMP DEFAULT now()
);
```

Here's how the **statement** looks in **SQL Shell (psql)**. ```CREATE TABLE``` message indicates that the **table** was created succesfully.

```
testdb=# CREATE TABLE cars (
testdb(#     id SERIAL PRIMARY KEY,
testdb(#     brand TEXT NOT NULL,
testdb(#     model TEXT NOT NULL,
testdb(#     year INT,
testdb(#     created_at TIMESTAMP DEFAULT now()
testdb(# );
CREATE TABLE
testdb=#
```

We can use ```\dt``` **psql meta-command** to list all tables in the current database/schema.

```
testdb=# \dt
          List of tables
 Schema | Name | Type  |  Owner
--------+------+-------+----------
 public | cars | table | postgres
(1 row)
```

Everything looks in order, so now we can **insert** some cars into the ```cars``` table:

- ```INSERT``` tells the database that data will be inserted into a table, ```INTO``` specifies which table (```cars``` in this case) you want to insert the row(s) into, ```(brand, model, year)``` tells **SQL** which columns you are providing data for in the new row, and ```VALUES``` introduces the actual data you want to insert.
- **Single quotes** ```''``` are used for the values of ```brand``` and ```model``` because they are **text**/**strings**, and no quotes are used for ```year``` because they are **numbers** (**integer**, **serial**, **float**, etc.).

```
INSERT INTO cars (brand, model, year) VALUES
('Lancia', 'Delta S4', 1986),
('Volvo', 'V70', 1997),
('Toyota', 'Celica', 1994);
```

The output looks as follows:

```
testdb=# INSERT INTO cars (brand, model, year) VALUES
testdb-# ('Lancia', 'Delta S4', 1986),
testdb-# ('Volvo', 'V70', 1997),
testdb-# ('Toyota', 'Celica', 1994);
INSERT 0 3
testdb=#
```

Data insertion was successful based on ```INSERT 0 3``` message. ```INSERT``` = the statement worked, ```0``` = number of rows affected for **special columns** like **sequences** (ignore), and ```3``` = 3 rows were successfully added.

We can use a **SELECT** statement to read data from a table. ```SELECT``` tells **SQL** which columns you want to see, **asterisk** ```*``` = all columns, ```FROM cars``` = from the ```cars``` table, and the **semicolon** ```;``` ends the **SQL** statement as per usual. Though, in real projects, explicitly selecting columns is usually preferred.

```
SELECT * FROM cars;
```

I get the following output:

```
testdb=# SELECT * FROM cars;
 id | brand  |  model   | year |         created_at
----+--------+----------+------+----------------------------
  1 | Lancia | Delta S4 | 1986 | 2026-01-27 17:59:52.763842
  2 | Volvo  | V70      | 1997 | 2026-01-27 17:59:52.763842
  3 | Toyota | Celica   | 1994 | 2026-01-27 17:59:52.763842
(3 rows)
```

The statement worked as intended. If we want to, we can also select specific columns like this: ```SELECT brand, model FROM cars;```:

```
testdb=# SELECT brand, model FROM cars;
 brand  |  model
--------+----------
 Lancia | Delta S4
 Volvo  | V70
 Toyota | Celica
(3 rows)
```

Or we can, for example, only show cars made before **1995** like this: ```SELECT * FROM cars WHERE year < 1995;```. ```WHERE``` = only return **rows** that match this condition, ```year``` = the column name in my ```cars``` table, ```<``` = less than, and ```1995``` = the value to compare against:

```
testdb=# SELECT * FROM cars WHERE year < 1995;
 id | brand  |  model   | year |         created_at
----+--------+----------+------+----------------------------
  1 | Lancia | Delta S4 | 1986 | 2026-01-27 17:59:52.763842
  3 | Toyota | Celica   | 1994 | 2026-01-27 17:59:52.763842
(2 rows)
```

With ```SELECT * FROM cars ORDER BY year DESC;``` we can sort cars from newest to oldest. ```ORDER BY``` tells **SQL** to sort the result rows (but it only affects how results are displayed, not how they are stored), ```year``` = the **column** used for sorting, and ```DESC``` = descending order meaning largest value first and smallest value last (the opposite of **DESC** is **ASC** for **ascending** order):

```
testdb=# SELECT * FROM cars ORDER BY year DESC;
 id | brand  |  model   | year |         created_at
----+--------+----------+------+----------------------------
  2 | Volvo  | V70      | 1997 | 2026-01-27 17:59:52.763842
  3 | Toyota | Celica   | 1994 | 2026-01-27 17:59:52.763842
  1 | Lancia | Delta S4 | 1986 | 2026-01-27 17:59:52.763842
(3 rows)
```

We can now conclude that the **PostgreSQL** environment works correctly, so we can move onto bigger projects. Let's delete the ```cars``` table with ```DROP TABLE cars;``` statement:

```
postgres=# \c testdb
You are now connected to database "testdb" as user "postgres".
testdb=# DROP TABLE cars;
DROP TABLE
testdb=#
```

```DROP TABLE``` message informs us that the statement was executed successfully. We can also confirm this further with the ```\dt``` **psql meta-command**:

```
testdb=# \dt
Did not find any tables.
testdb=#
```

Let's also delete the whole ```testdb``` database. First we need to connect back to the root ```postgres```, after which we use ```DROP DATABASE testdb;``` statement to delete the database and all its possible tables *permanently* (you cannot undo this):

```
testdb=# \c postgres
You are now connected to database "postgres" as user "postgres".
postgres=# DROP DATABASE testdb;
DROP DATABASE
postgres=#
```

```DROP DATABASE``` message informs us that the statement was executed successfully. We can also confirm this further with the ```\l``` **psql meta-command**, which lists all databases:

```
postgres=# \l
                                                List of databases
   Name    |  Owner   | Encoding | Locale Provider | Collate | Ctype | Locale | ICU Rules |   Access privileges
-----------+----------+----------+-----------------+---------+-------+--------+-----------+-----------------------
 postgres  | postgres | UTF8     | libc            | fi-FI   | fi-FI |        |           |
 template0 | postgres | UTF8     | libc            | fi-FI   | fi-FI |        |           | =c/postgres          +
           |          |          |                 |         |       |        |           | postgres=CTc/postgres
 template1 | postgres | UTF8     | libc            | fi-FI   | fi-FI |        |           | =c/postgres          +
           |          |          |                 |         |       |        |           | postgres=CTc/postgres
(3 rows)
```

```testdb``` does not exist anymore. Finally, we can close **SQL Shell (psql)** terminal with ```\q``` **psql meta-command**:

```
postgres=# \q
Press any key to continue . . .
```

This concludes the installation, setup, and testing.


## Creating a Movie Rating Database

I'm an avid [IMDb](https://www.imdb.com/) user, and I usually rate every movie I see. I really enjoy using **IMDb**, but the only issue with it is that you can only rate movies with whole numbers (**7/10**, **8/10**, etc.) whereas I'd like to be able to use decimals (**7.5/10**, **8.5/10**, etc.). For this reason I think creating a database for my own movie ratings could be a fun and even useful idea. I believe that a good way to learn is to build something you actually find enjoyable and useful.

These are my initial ideas for the database:

- Create a **database** called ```movie_database```. This name follows accepted **SQL** naming conventions (**lowercase**, **underscores**). ✅
- Within ```movie_database```, create a **table** called ```movie_ratings``` with the following columns: ```id```, ```title```, ```year```, ```rating```, and ```rated_at```. ✅
- Perhaps the ability to import movie ratings from **CSV files** (**comma-separated values**), because writing them manually to **SQL statements** can be cumbersome.
- Ratings are allowed between **0.5/10** and **10/10** with **0.5** increments, meaning e.g. **7.7/10** is invalid. ✅

This is the **schema** (defines the **columns**, their **data types**, and constraints) for ```movie_ratings``` **table**:

| Column | Type | Description |
|-|-|-|
| ```id``` | ```INT GENERATED ALWAYS AS IDENTITY``` | Unique identifier, primary key |
| ```title``` | ```TEXT``` | Movie title |
| ```year``` | ```INT``` | Release year |
| ```rating``` | ```NUMERIC(3,1)``` | Movie rating (allows decimals, e.g., 7.5) |
| ```rated_at``` | ```TIMESTAMP``` | Automatic timestamp when the row is added |

I'll be using ```INT GENERATED ALWAYS AS IDENTITY``` for ```id```, because ```SERIAL``` is considered a legacy and ```IDENTITY``` is the modern, standard-compliant way to auto-generate unique numbers in **PostgreSQL** (Other **SQL** databases have their own ways to auto-generate **IDs**):

- ```INT``` = **integer** or a **whole number**
- ```GENERATED``` indicates that **PostgreSQL** will generate values automatically for this column
- ```ALWAYS``` specifies that you cannot manually provide a value for this column
- ```AS IDENTITY``` tells **PostgreSQL** to use the standard-compliant way to generate sequential numbers for the column

I start the execution of this project by logging into my root user in **SQL Shell (psql)** and by creating the ```movie_database```:

```
postgres=# CREATE DATABASE movie_database;
CREATE DATABASE
```

Then I draft the **schema** for ```movie_ratings``` **table** in **Notepad++**. I add a ```CHECK``` **constraint** last minute to prevent invalid ratings, so that you can't accidentally insert a rating below **0** or above **10**, and only **0.5** increments are allowed. A more specific explanation:

- ```NUMERIC``` (or ```DECIMAL```) is a precise numeric type, suitable for decimal numbers.
- ```(3,1)``` are precision and scale: ```3``` = total number of digits allowed including digits before and after the decimal point (**10.0** is the highest rating) and ```1``` = number of digits allowed after the decimal point (the fractional part).
- ```CHECK ()``` is a constraint that validates data before insertion and enforces all rules inside its **parentheses**.
- ```rating >= 0.5``` means the ```rating``` has to be **greater than or equal to** ```>=``` the minimum allowed value (**0.5**).
- In ```AND rating <= 10```: ```AND``` = a **logical operator** in **SQL** that combines multiple conditions and ```rating <= 10``` means the ```rating``` has to be **less than or equal to** ```<=``` the maximum allowed value (**10**).
- ```AND (rating * 2) = FLOOR(rating * 2)``` makes sure the ratings are in **0.5 increments**. ```(rating * 2)``` multiplies the rating by **2**, ```=``` or **equality operator** compares these **mathematical expressions**, and in ```FLOOR(rating * 2)``` the ```FLOOR(x)``` rounds down to the nearest whole number. For example: **(6.7 * 2) = 13.4** and **FLOOR(6.7 * 2) = 13**, and therefore **13.4 = 13** is **not true** and will fail the check. However, for example: **(6.5 * 2) = 13** and **FLOOR(6.5 * 2) = 13**, and therefore **13 = 13** is **true** and allowed.

```
CREATE TABLE movie_ratings (
	id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
	title TEXT NOT NULL,
	year INT,
	rating NUMERIC(3,1) CHECK (
		rating >= 0.5
		AND rating <= 10
		AND (rating * 2) = FLOOR(rating * 2)
	),
	rated_at TIMESTAMP DEFAULT now()
);
```

Now I'm ready to test this out in practice:

```
You are now connected to database "movie_database" as user "postgres".
movie_database=# CREATE TABLE movie_ratings (
movie_database(# id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
movie_database(# title TEXT NOT NULL,
movie_database(# year INT,
movie_database(# rating NUMERIC(3,1) CHECK (
movie_database(# rating >= 0.5
movie_database(# AND rating <= 10
movie_database(# AND (rating * 2) = FLOOR(rating * 2)
movie_database(# ),
movie_database(# rated_at TIMESTAMP DEFAULT now()
movie_database(# );
CREATE TABLE
movie_database=#
```

Success! Now I can insert a few movies I've lately watched and rated, though first I draft the statement in **Notepad++**:

```
INSERT INTO movie_ratings (title, year, rating) VALUES
('The Last Samurai', 2003, 9.5),
('Heat', 1995, 9),
('Good Will Hunting', 1997, 8.5),
('Ad Astra', 2019, 8);
```

Let's test the statement:

```
movie_database=# INSERT INTO movie_ratings (title, year, rating) VALUES
movie_database-# ('The Last Samurai', 2003, 9.5),
movie_database-# ('Heat', 1995, 9),
movie_database-# ('Good Will Hunting', 1997, 8.5),
movie_database-# ('Ad Astra', 2019, 8);
INSERT 0 4
movie_database=#
```

No errors, so let's see how it looks:

```
movie_database=# SELECT * FROM movie_ratings;
                           movie_database
 id |       title       | year | rating |          rated_at
----+-------------------+------+--------+----------------------------
  1 | The Last Samurai  | 2003 |    9.5 | 2026-01-28 21:35:27.981159
  2 | Heat              | 1995 |    9.0 | 2026-01-28 21:35:27.981159
  3 | Good Will Hunting | 1997 |    8.5 | 2026-01-28 21:35:27.981159
  4 | Ad Astra          | 2019 |    8.0 | 2026-01-28 21:35:27.981159
(4 rows)
```

Perfect! I even coincidentally inserted the ratings in a descending order, even though there's a statement for it:

```
movie_database=# SELECT * FROM movie_ratings ORDER BY rating DESC;
                           movie_database
 id |       title       | year | rating |          rated_at
----+-------------------+------+--------+----------------------------
  1 | The Last Samurai  | 2003 |    9.5 | 2026-01-28 21:35:27.981159
  2 | Heat              | 1995 |    9.0 | 2026-01-28 21:35:27.981159
  3 | Good Will Hunting | 1997 |    8.5 | 2026-01-28 21:35:27.981159
  4 | Ad Astra          | 2019 |    8.0 | 2026-01-28 21:35:27.981159
(4 rows)
```

I want to see what happens if I insert a movie with a rating that is not allowed:

```
movie_database=# INSERT INTO movie_ratings (title, year, rating) VALUES
movie_database-# ('The Tender Bar', 2021, 7.2);
ERROR:  new row for relation "movie_ratings" violates check constraint "movie_ratings_rating_check"
DETAIL:  Failing row contains (6, The Tender Bar, 2021, 7.2, 2026-01-28 21:48:29.13976).
```

The fail check works as intended. Now let's insert with the correct rating:

```
movie_database=# INSERT INTO movie_ratings (title, year, rating) VALUES
movie_database-# ('The Tender Bar', 2021, 7);
INSERT 0 1
movie_database=#
```

Let's see how the ```movie_ratings``` table looks now:

```
movie_database=# SELECT * FROM movie_ratings ORDER BY rating DESC;
                           movie_database
 id |       title       | year | rating |          rated_at
----+-------------------+------+--------+----------------------------
  1 | The Last Samurai  | 2003 |    9.5 | 2026-01-28 21:35:27.981159
  2 | Heat              | 1995 |    9.0 | 2026-01-28 21:35:27.981159
  3 | Good Will Hunting | 1997 |    8.5 | 2026-01-28 21:35:27.981159
  4 | Ad Astra          | 2019 |    8.0 | 2026-01-28 21:35:27.981159
  7 | The Tender Bar    | 2021 |    7.0 | 2026-01-28 21:48:40.23575
(5 rows)
```

What is notable is that the ```id``` of ```The Tender Bar``` is ```7```, because I made **2** failed row insertions before it. In **PostgreSQL**, when you use ```INT GENERATED ALWAYS AS IDENTITY``` (or ```SERIAL```), the **ID counter** keeps increasing even if a row insertion fails. This behavior is completely standard.
