# Performing a Conditional Search

The database operations team has created a relational database named **world** containing three tables: **city**, **country**, and **countrylanguage**. 
To help the team, I will write a few queries to search for records in the country table by using the **SELECT** statement and a **WHERE** clause.



## Task 1: Connect to the Command Host
I connect to the Command Host EC2 instance using the Session Manager tab. Then on the terminal I run the commands to connect to the database:
```bash
sudo su
cd /home/ec2-user/
mysql -u root --password='re:St@rt!9'
```

## Task 2: Query the world database
There are 4 databases.
```sql
MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| world              |
+--------------------+
4 rows in set (0.001 sec)
```
Here I query the world database by using various SELECT statements and database functions.
```sql
SELECT * FROM world.country;
-- returns the rows in the country table: total of 239 rows

SELECT Name, Capital, Region, SurfaceArea, Population FROM world.country WHERE Population >= 50000000 AND Population <= 100000000;
-- returns the rows in the country table: total of 14 rows

SELECT Name, Capital, Region, SurfaceArea, Population FROM world.country WHERE Population BETWEEN 50000000 AND 100000000;
-- same as before but with the between operator

SELECT sum(Population) from world.country WHERE Region LIKE "%Europe%";
-- returns the population of all European countries: 634947800 rows

SELECT sum(population) as "Europe Population Total" from world.country WHERE region LIKE "%Europe%";
-- return the same information as the previous query with the column alias

SELECT Name, Capital, Region, SurfaceArea, Population from world.country WHERE LOWER(Region) LIKE "%central%";
-- perform a case-sensitive search by using the LOWER function: total of 31 rows
```

## Challenge
Write a query to return the sum of the surface area and sum of the population of North America.
```sql
MariaDB [(none)]> SELECT SUM(SurfaceArea) as "N. America Surface Area", SUM(Population) as "N. America Population" FROM world.country WHERE Region = "North America";
+-------------------------+-----------------------+
| N. America Surface Area | N. America Population |
+-------------------------+-----------------------+
|             21500515.00 |             309632000 |
+-------------------------+-----------------------+
1 row in set (0.000 sec)
```
