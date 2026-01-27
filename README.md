# SQL

A project for learning the basics of **Structured Query Language**. My purpose is to learn to use **SQL** in a way that is applicable to real-life projects and work.


## Table of Contents

- [Software Installation and Setup](https://github.com/Pece17/SQL/blob/main/README.md#software-installation-and-setup)


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

Now I can test if the database works correctly. The ```SELECT``` statement is used to select data from a database, so I enter the following command to check the database version:

```
postgres=# SELECT version();
                                 version
-------------------------------------------------------------------------
 PostgreSQL 18.1 on x86_64-windows, compiled by msvc-19.44.35221, 64-bit
(1 row)
```

Next let's create a new database called ```testdb```, so that we avoid working in the default ```postgres``` database. The ```CREATE DATABASE``` statement is used to create a new **SQL** database. **Semicolons** (**;**) are also used after statements in **SQL**:

```
postgres=# CREATE DATABASE testdb;
CREATE DATABASE
postgres=#
```

Now we can connect to the newly created database using the ```\c``` command:

```
postgres=# \c testdb
You are now connected to database "testdb" as user "postgres".
testdb=#
```

Let's create our first table in the ```testdb``` database with the ```CREATE TABLE``` statement:

- ```cars``` is the name of the table.
- ```id``` is the **column** name for the unique identifier for each car, ```SERIAL``` automatically generates numbers for new rows, and ```PRIMARY KEY``` ensures this column is unique and cannot be empty (```NULL```).
- ```brand``` is the **column** name for the brand of the car (Volvo, Toyota, etc.), ```TEXT``` is the data type for any length of string/text, and ```NOT NULL``` means this field must have a value and you cannot insert a **row** without a brand.
- ```model``` is the **column** name for the model of the car (V70, Celica, etc.), and ```TEXT``` and ```NOT NULL``` work similarly as in the previous **row**.
- ```year``` is the **column** name for the manufacturing year of the car and ```INT``` is the data type for **integers** (whole numbers).
- Each **column** definition (not **row**) is separated by a **comma** (**,**).

```
CREATE TABLE cars (
    id SERIAL PRIMARY KEY,           -- Unique ID for each car
    brand TEXT NOT NULL,             -- Car brand, e.g., Toyota, Volvo
    model TEXT NOT NULL,             -- Car model, e.g., Corolla, V60
    year INT,                        -- Year of manufacture
    created_at TIMESTAMP DEFAULT now() -- Auto timestamp of row creation
);
```
