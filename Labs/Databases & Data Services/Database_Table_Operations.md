# Database Table Operations

In this lab I will practice creating and dropping (deleting) databases and 
tables previopusly configured by a database operations team.



## Task 1: Connect to the Commmand Host EC2 instance configured with a database client
Here I connect to the Commmand Host using the Session Manager tab. Then on terminal I conncet to the database.
sudo su
cd /home/ec2-user/
mysql -u root --password='re:St@rt!9'
```


Output:
```bash
sh-4.2$ sudo su
[root@ip-10-1-11-159 bin]# cd /home/ec2-user/
[root@ip-10-1-11-159 ec2-user]# mysql -u root --password='re:St@rt!9'
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 10.5.29-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

## Task 2: Create a database and a table
Here I create a database named **world** and a table named **country**. Then I alter the country table.

```mariaDB
MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.012 sec)

MariaDB [(none)]> CREATE DATABASE world;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| world              |
+--------------------+
4 rows in set (0.000 sec)

MariaDB [(none)]> CREATE TABLE world.country (
    ->   `Code` CHAR(3) NOT NULL DEFAULT '',
    ->   `Name` CHAR(52) NOT NULL DEFAULT '',
    ->   `Conitinent` enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South  America') NOT NULL DEFAULT 'Asia',
    ->   `Region` CHAR(26) NOT NULL DEFAULT '',
    ->   `SurfaceArea` FLOAT(10,2) NOT NULL DEFAULT '0.00',
    ->   `IndepYear` SMALLINT(6) DEFAULT NULL,
    ->   `Population` INT(11) NOT NULL DEFAULT '0',
    ->   `LifeExpectancy` FLOAT(3,1) DEFAULT NULL,
    ->   `GNP` FLOAT(10,2) DEFAULT NULL,
    ->   `GNPOld` FLOAT(10,2) DEFAULT NULL,
    ->   `LocalName` CHAR(45) NOT NULL DEFAULT '',
    ->   `GovernmentForm` CHAR(45) NOT NULL DEFAULT '',
    ->   `HeadOfState` CHAR(60) DEFAULT NULL,
    ->   `Capital` INT(11) DEFAULT NULL,
    ->   `Code2` CHAR(2) NOT NULL DEFAULT '',
    ->   PRIMARY KEY (`Code`)
    -> );
Query OK, 0 rows affected (0.036 sec)

MariaDB [(none)]> USE world;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [world]> SHOW TABLES;
+-----------------+
| Tables_in_world |
+-----------------+
| country         |
+-----------------+
1 row in set (0.000 sec)

MariaDB [world]> SHOW COLUMNS FROM world.country;
+----------------+----------------------------------------------------------------------------------------+------+-----+---------+-------+
| Field          | Type                                                                                   | Null | Key | Default | Extra |
+----------------+----------------------------------------------------------------------------------------+------+-----+---------+-------+
| Code           | char(3)                                                                                | NO   | PRI |         |       |
| Name           | char(52)                                                                               | NO   |     |         |       |
| Conitinent     | enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South  America') | NO   |     | Asia    |       |
| Region         | char(26)                                                                               | NO   |     |         |       |
| SurfaceArea    | float(10,2)                                                                            | NO   |     | 0.00    |       |
| IndepYear      | smallint(6)                                                                            | YES  |     | NULL    |       |
| Population     | int(11)                                                                                | NO   |     | 0       |       |
| LifeExpectancy | float(3,1)                                                                             | YES  |     | NULL    |       |
| GNP            | float(10,2)                                                                            | YES  |     | NULL    |       |
| GNPOld         | float(10,2)                                                                            | YES  |     | NULL    |       |
| LocalName      | char(45)                                                                               | NO   |     |         |       |
| GovernmentForm | char(45)                                                                               | NO   |     |         |       |
| HeadOfState    | char(60)                                                                               | YES  |     | NULL    |       |
| Capital        | int(11)                                                                                | YES  |     | NULL    |       |
| Code2          | char(2)                                                                                | NO   |     |         |       |
+----------------+----------------------------------------------------------------------------------------+------+-----+---------+-------+
15 rows in set (0.004 sec)

MariaDB [world]> ALTER TABLE world.country RENAME COLUMN Conitinent TO Continent;
Query OK, 0 rows affected (0.007 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [world]> SHOW COLUMNS FROM world.country;
+----------------+----------------------------------------------------------------------------------------+------+-----+---------+-------+
| Field          | Type                                                                                   | Null | Key | Default | Extra |
+----------------+----------------------------------------------------------------------------------------+------+-----+---------+-------+
| Code           | char(3)                                                                                | NO   | PRI |         |       |
| Name           | char(52)                                                                               | NO   |     |         |       |
| Continent      | enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South  America') | NO   |     | Asia    |       |
| Region         | char(26)                                                                               | NO   |     |         |       |
| SurfaceArea    | float(10,2)                                                                            | NO   |     | 0.00    |       |
| IndepYear      | smallint(6)                                                                            | YES  |     | NULL    |       |
| Population     | int(11)                                                                                | NO   |     | 0       |       |
| LifeExpectancy | float(3,1)                                                                             | YES  |     | NULL    |       |
| GNP            | float(10,2)                                                                            | YES  |     | NULL    |       |
| GNPOld         | float(10,2)                                                                            | YES  |     | NULL    |       |
| LocalName      | char(45)                                                                               | NO   |     |         |       |
| GovernmentForm | char(45)                                                                               | NO   |     |         |       |
| HeadOfState    | char(60)                                                                               | YES  |     | NULL    |       |
| Capital        | int(11)                                                                                | YES  |     | NULL    |       |
| Code2          | char(2)                                                                                | NO   |     |         |       |
+----------------+----------------------------------------------------------------------------------------+------+-----+---------+-------+
15 rows in set (0.001 sec)

MariaDB [world]>
```

**Note**: To store data in a database, the database needs to contain one or more tables. In an SQL database, a table needs a 
well-defined structure, known as a table **schema**.

## Challenge 1
Create a table named city and add two columns named Name and Region. Both columns should use the CHAR data type.

```bash
MariaDB [world]> CREATE TABLE world.city (`Name` CHAR(52), `Region` CHAR(26));
Query OK, 0 rows affected (0.008 sec)

MariaDB [world]> SHOW TABLES;
+-----------------+
| Tables_in_world |
+-----------------+
| city            |
| country         |
+-----------------+
2 rows in set (0.000 sec)

MariaDB [world]>
```

## Task 3: Delete a database and tables
Here I delete the **city** table. 

```bash
MariaDB [world]> DROP TABLE world.city;
Query OK, 0 rows affected (0.005 sec)

MariaDB [world]> SHOW TABLES;
+-----------------+
| Tables_in_world |
+-----------------+
| country         |
+-----------------+
1 row in set (0.000 sec)
```

## Challenge 2
1. Delete the **country** table. 
```bash
MariaDB [world]> DROP TABLE world.country;
Query OK, 0 rows affected (0.005 sec)

MariaDB [world]> SHOW TABLES;
Empty set (0.000 sec)
```

2. Delete the **world** database.
```bash
MariaDB [world]> DROP DATABASE world;
Query OK, 0 rows affected (0.005 sec)

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.000 sec)
```

## Conclusion
- Used the **CREATE** statement to create databases and tables
- Used the **SHOW** statement to view available databases and tables
- Used the **ALTER** statement to alter the structure of a table
- Used the **DROP** statement to delete databases and tables

## MySQL command-line client
1. The MySQL command-line client is an SQL shell that you can use to interact with database engines.

| Switch             | Description                                                     |
|--------------------|-----------------------------------------------------------------|
| -u or --user       | The MySQL user name used to connect to a database instance      |
| -p or --password   | The MySQL password used to connect to a database instance       |

Example: Run `mysql -u root --password='<psswd>` to connect to the relational database instance
