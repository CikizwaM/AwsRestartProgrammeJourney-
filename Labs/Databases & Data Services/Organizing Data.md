# Organizing Data

The database operations team has created a relational database named **world** containing three tables: city, country, and countrylanguage. 
I will write a few queries to group records for analysis by using both the **GROUP BY** and **OVER** clauses.



## Task 1: Connect to the Command Host
I connect to the Command Host instance using the Session Manager tab. The instance contains a database client.
```bash
sudo su
cd /home/ec2-user/
mysql -u root --password='re:St@rt!9'
```

## Task 2: Query the world database
Here I query the **world** database using various **SELECT** statements and database functions.
```sql
-- review the table schema, data, and number of rows in the country table
SELECT * FROM world.country;      -- 239 rows

-- list of records where the Region is Australia and New Zealand, the results are arranges by Population in descending order
SELECT Region, Name, Population FROM world.country WHERE Region = 'Australia and New Zealand' ORDER By Population desc;
+---------------------------+-------------------------+------------+
| Region                    | Name                    | Population |
+---------------------------+-------------------------+------------+
| Australia and New Zealand | Australia               |   18886000 |
| Australia and New Zealand | New Zealand             |    3862000 |
| Australia and New Zealand | Christmas Island        |       2500 |
| Australia and New Zealand | Norfolk Island          |       2000 |
| Australia and New Zealand | Cocos (Keeling) Islands |        600 |
+---------------------------+-------------------------+------------+

-- use the GROUP BY clause to group related records together.
SELECT Region, SUM(Population) FROM world.country WHERE Region = 'Australia and New Zealand' GROUP By Region ORDER By SUM(Population) desc;
+---------------------------+-----------------+
| Region                    | SUM(Population) |
+---------------------------+-----------------+
| Australia and New Zealand |        22753100 |
+---------------------------+-----------------+

-- uses a windowing function to generate a running total by adding the Population of the first record to the Population of the second record and subsequent records
SELECT Region, Name, Population, SUM(Population) OVER(partition by Region ORDER BY Population) as 'Running Total' FROM world.country WHERE Region = 'Australia and New Zealand';
+---------------------------+-------------------------+------------+---------------+
| Region                    | Name                    | Population | Running Total |
+---------------------------+-------------------------+------------+---------------+
| Australia and New Zealand | Cocos (Keeling) Islands |        600 |           600 |
| Australia and New Zealand | Norfolk Island          |       2000 |          2600 |
| Australia and New Zealand | Christmas Island        |       2500 |          5100 |
| Australia and New Zealand | New Zealand             |    3862000 |       3867100 |
| Australia and New Zealand | Australia               |   18886000 |      22753100 |
+---------------------------+-------------------------+------------+---------------+

-- groups the records by Region and orders them by Population with the OVER() clause. This query also includes the RANK() function to generate a rank number indicating the position of each record in the result set.
SELECT Region, Name, Population, SUM(Population) OVER(partition by Region ORDER BY Population) as 'Running Total', RANK() over(partition by region ORDER BY population) as 'Ranked' FROM world.country WHERE region = 'Australia and New Zealand';
+---------------------------+-------------------------+------------+---------------+--------+
| Region                    | Name                    | Population | Running Total | Ranked |
+---------------------------+-------------------------+------------+---------------+--------+
| Australia and New Zealand | Cocos (Keeling) Islands |        600 |           600 |      1 |
| Australia and New Zealand | Norfolk Island          |       2000 |          2600 |      2 |
| Australia and New Zealand | Christmas Island        |       2500 |          5100 |      3 |
| Australia and New Zealand | New Zealand             |    3862000 |       3867100 |      4 |
| Australia and New Zealand | Australia               |   18886000 |      22753100 |      5 |
+---------------------------+-------------------------+------------+---------------+--------+
```

## Challenge
Write a query to rank the countries in each region by their population from largest to smallest.

You have to determine whether to use either the GROUP BY or OVER grouping clause and either the SUM() or RANK() function.

```sql
SELECT Region, Name, Population, RANK() OVER(partition by Region ORDER BY Population desc) as 'Ranked' FROM world.country order by Region, Ranked;
+---------------------------+----------------------------------------------+------------+--------+
| Region                    | Name                                         | Population | Ranked |
+---------------------------+----------------------------------------------+------------+--------+
| Antarctica                | French Southern territories                  |          0 |      1 |
| Antarctica                | Bouvet Island                                |          0 |      1 |
| Antarctica                | South Georgia and the South Sandwich Islands |          0 |      1 |
| Antarctica                | Antarctica                                   |          0 |      1 |
| Antarctica                | Heard Island and McDonald Islands            |          0 |      1 |
| Australia and New Zealand | Australia                                    |   18886000 |      1 |
| Australia and New Zealand | New Zealand                                  |    3862000 |      2 |
| Australia and New Zealand | Christmas Island                             |       2500 |      3 |
| Australia and New Zealand | Norfolk Island                               |       2000 |      4 |
| Australia and New Zealand | Cocos (Keeling) Islands                      |        600 |      5 |
....
```

## Conclusion
- I used the **GROUP BY** clause with the aggregate function **SUM()**
- I used the **OVER** clause with the **RANK()** window function
- I used the **OVER** clause with the aggregate function **SUM()** and the **RANK()** window function
