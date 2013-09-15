---
author: sunilkumarn
comments: true
date: 2010-10-18 18:23:31+00:00
layout: post
slug: python-db-api
title: Python DB-API
wordpress_id: 363
categories:
- Python
---

Python is one of the more popular Open Source programming languages, owing largely to its own native expressiveness as well as to the variety of support modules that are available to extend its capabilities. One of these modules is DB-API, which, as the name implies, provides a database application programming interface. We will have a discussion about how to connect and use a General purpose database system like Mysql with Python. The DB API provides a minimal standard for working with databases, using Python structures and syntax wherever possible .

4 Steps of the Python DB-API includes the following:


1) Importing the API module. 

2) Acquiring a connection with the database.

3) Issuing SQL statements and stored procedures.

4) Closing the connection
There are lots of database systems that are available .Here we use Mysql as our database system .You could also use database systems like PostgreSQL, SQL Server etc. Python's DB-API uses a two-level architecture in which the top level provides an abstract interface that is similar for all supported database engines, and a lower level consisting of drivers for specific engines that handle engine-dependent details. This means, of course, that to use DB-API for writing Python scripts, you must have a driver for your particular database system. Since we have  fixed our database system as MySQL, DB-API provides database access by means of the MySQLdb driver . Therefore we need a module **MySQLdb** in Python '' .

At first , we need to get Mysql installed in our system.

**$: apt-get install mysql-server-5.0**

Just try and enter into the interactive prompt of mysql .

** $: mysql -u 'user' -p**

You will get your mysql interactive prompt where you could just brush up your sql knowledge.


_**Step 1) Importing the API module.**_


You need to make sure that you that have MySQLdb installed on your machine.

Do this:

**$: python**

**>>>import MySQLdb**

Obviously if you dont have it installed , you would be getting this :

**Traceback (most recent call last):**

**File "<stdin>", line 1, in <module>**

**ImportError: No module named MySQLdb**

To install MySQLdb module, download it from [MySQLdb Download](http://sourceforge.net/projects/mysql-python) page and proceed as follows:

    
    $ gunzip MySQL-python-1.2.2.tar.gz
    $ tar -xvf MySQL-python-1.2.2.tar
    $ cd MySQL-python-1.2.2
    $ python setup.py build
    $ python setup.py install


Now do

**$ : python**

**>>>import MySQLdb**

**>>>**

We have done with Step 1) out of the 4 steps of the Python DB-API .

Now we move into step 2). The prerequisite for performing step 2) of the DB-API is that you need to create a database ( 'Persons' in our examples to follow ) in mysql .

You could do this by :

**$: mysql -u 'user' -p**

Enter  mysql .

Do :

**mysql>CREATE DATABASE Persons;**

**mysql>Query OK, 1 row affected (0.03 sec)**

Once you have done with this


_**Step 2 ) Acquiring a connection with the database.**_





_conn = MySQLdb.connect (host = "localhost",_

_user = "user",_

_passwd = "asdfgh",_

_db = "Persons")_

Here we open a database connection using the 'connect' function of the MySQLdb module.

The four parameters that are passed to the function 'connect'  are :


**host** --       the system in which the database is created


**user** --       the user who is trying to acquire a connection with the database ( the user must be authenticated with mysql if the attempt to create a connection needs to be successful.

**passwd** --      the password of the mysql database system .

**db** --      the name of the database created using mysql  .

If the 'connect()' call succeeds, it returns a connection object that serves as the basis for further interaction with MySQL( here 'conn'). If the call fails, it raises an exception .Therefore you better perform exception handling so that you get to know more about the root cause of the error.

_try:_

_conn = MySQLdb.connect (host = "localhost",_

_user = "root",_

_passwd = "asdfgh",_

_db = "Persons")_

_except MySQLdb.Error,e:_

_print "Error occured ",e.args[0],e.args[1]_

_sys.exit (1)_

_
_

Here on an unsuccessful attempt to make a connection , an exception is raised ( MySQLdb.Error ) , the information about the error is stored in e.args[1] .




_**Step 3) Issuing SQL statements and stored procedures.**_


Now we need to create a cursor object for the connection between Python and mysql so that all the functions performed by sql are performed within from Python now . We create a cursor object using the ' cursor()' method .

We are all set to execute sql statements in an Object oriented environment , and thats using the 'execute()' function . Any sql statement can be called in from Python using the 'execute()' function.

Consider you want to create a new table in our database 'Persons' .

_cursor.execute("""_

_CREATE TABLE PERSON_

_(_

_ID               int,_

_lastname     CHAR(40),_

_firstname    CHAR(40),_

_City      CHAR(40)_

_)_

_""")_

You have created a table named 'PERSON ' with fields as mentioned above .

As you would imagine , any other statements could be performed as such , for eg.

_cursor.execute ("""_

_INSERT INTO PERSON (ID, lastname,firstname,City)_

_VALUES_

_(1,'KUMAR','SUNIL','KANNUR'),_

_(2,'JOHN','HARI','KOTTAYAM')_

_""")_

or

_cursor.execute ("SELECT * FROM PERSON WHERE City like 'K%'")_

and any of the sql statements .


_** Step 4) Closing the connection**_


_conn.close()_

Here we have terminated the connection after all our need with the DB-API has been done with . The DB-API allows us to update , insert , retrieve and modify within a database in a simple and elegant way .

We have tried and demonstrated a simple example of how to use the Python DB-API .

Using Python with Sql,we have found an object-relational mapping where the object-oriented programming paradigm of Python meets the relational paradigm of Sql. An object-relational mapping is a bridge between the two worlds. As we saw , it lets us define classes that correspond to the tables of a database. Later , we use methods on those classes and their instances to interact with the database .
