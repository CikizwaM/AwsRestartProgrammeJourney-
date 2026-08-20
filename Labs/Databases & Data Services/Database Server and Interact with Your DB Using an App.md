# Build Your DB Server and Interact With Your DB Using an App

In this lab I will leverage an AWS-managed database instance by solving relational database needs.

**Amazon Relational Database Service** (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud. 
It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, which allows to focus 
on the applications and business. Amazon RDS provides with six familiar database engines to choose from: 
Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL and MariaDB.



# Task 1 — Create RDS Security Group

## Objective

Create a security group that allows the web server to communicate
with the RDS MySQL database.

## Configuration

Security Group Name:
DB Security Group

Description:
Permit access from Web Security Group

VPC:
Lab VPC

## Inbound Rule

| Type | Protocol | Port | Source |
|---|---|---|---|
| MySQL/Aurora | TCP | 3306 | Web Security Group |

## Steps

1. Open the AWS VPC console.
2. Select Security Groups.
3. Select Create security group.
4. Enter the security group configuration.
5. Add the MySQL/Aurora inbound rule.
6. Select Web Security Group as the source.
7. Create the security group.

## Task 2: Create a DB Subnet Group
## Objective

Create a DB subnet group that tells Amazon RDS which subnets can be used for the database.

A DB subnet group must contain subnets from at least two Availability Zones.

## Configuration

DB Subnet Group Name:
DB Subnet Group

Description:
DB Subnet Group

VPC:
Lab VPC

## Subnets

| Subnet           | CIDR Block  | Type    |
| ---------------- | ----------- | ------- |
| Private Subnet 1 | 10.0.1.0/24 | Private |
| Private Subnet 2 | 10.0.3.0/24 | Private |

## Steps

1. Open the AWS Management Console.
2. Search for **RDS** and select **Aurora and RDS**.
3. Select **Subnet groups** from the left navigation pane.
4. Select **Create DB Subnet Group**.
5. Enter `DB Subnet Group` as the name.
6. Enter `DB Subnet Group` as the description.
7. Select **Lab VPC**.
8. Select the first and second Availability Zones.
9. Select subnet `10.0.1.0/24`.
10. Select subnet `10.0.3.0/24`.
11. Select **Create**.

## Result

The DB subnet group is created and can be used when launching the RDS database.

## Screenshot

## Task 3: Create an Amazon RDS DB Instance
## Objective

Create a Multi-AZ Amazon RDS MySQL database instance inside the Lab VPC.

The Multi-AZ deployment provides high availability by using a primary database and a standby database in a different Availability Zone.

## Configuration

DB Instance Identifier:
lab-db

Engine:
MySQL

Template:
Dev/Test

Availability and Durability:
Multi-AZ DB instance deployment (2 instances)

Master Username:
main

Instance Class:
db.t3.medium

Storage Type:
General Purpose SSD (gp3)

Allocated Storage:
20 GB

VPC:
Lab VPC

DB Subnet Group:
DB Subnet Group

Public Access:
No

VPC Security Group:
DB Security Group

Initial Database Name:
lab

## Steps

1. Open the AWS Management Console.
2. Search for **RDS** and select **Aurora and RDS**.
3. Select **Databases** from the left navigation pane.
4. Select the dropdown next to **Create database**.
5. Select **Full configuration**.
6. Select **MySQL** as the engine.
7. Select the **Dev/Test** template.
8. Select **Multi-AZ DB instance deployment (2 instances)**.
9. Enter `lab-db` as the DB instance identifier.
10. Enter `main` as the master username.
11. Configure the required master password.
12. Select **Burstable classes (includes t classes)**.
13. Select `db.t3.medium`.
14. Select **General Purpose SSD (gp3)**.
15. Set allocated storage to `20 GB`.
16. Under Connectivity, select **Don't connect to an EC2 compute resource**.
17. Select **Lab VPC**.
18. Select **DB Subnet Group**.
19. Set **Public access** to **No**.
20. Select **Choose existing** for the VPC security group.
21. Remove the default security group.
22. Select **DB Security Group**.
23. Disable **Enhanced monitoring**.
24. Disable **Performance Insights**.
25. Set the initial database name to `lab`.
26. Disable automated backups if required for the lab.
27. Select **Create database**.
28. Wait for the RDS database status to become **Available**.
29. Open `lab-db`.
30. Select **Connectivity & security**.
31. Copy the RDS endpoint.

## Security

The RDS database is configured with:

* No public access.
* Private subnets.
* DB Security Group.
* MySQL port 3306.
* Multi-AZ deployment.

Do not publish the database password or sensitive RDS information on GitHub.

## RDS Endpoint

Use a placeholder in the GitHub repository:

```text
[YOUR-RDS-ENDPOINT]
```

## Screenshot

## Task 4: Interact with Your Database
## Objective
I copy and past the WebServer IP address in a new broser tab:http://35.89.175.154

Connect the web application running on the web server to the Amazon RDS MySQL database.

The application uses the RDS database to store and retrieve Address Book information.

## Configuration

Endpoint:
[lab-db.c0egesud1gfd.us-west-2.rds.amazonaws.com]

Database:
lab

Username:
main

Password:
[REDACTED]

Port:
3306

## Steps

1. Open the AWS lab details.
2. Copy the **WebServer IP address**.
3. Open a new browser tab.
4. Enter the WebServer IP address.
5. Wait for the web application to load.
6. Select the **RDS** link at the top of the application.
7. Enter the RDS endpoint.
8. Enter `lab` as the database name.
9. Enter `main` as the username.
10. Enter the database password.
11. Select **Submit**.
12. Wait for the application to connect to the RDS database.
13. Verify that the Address Book application is displayed.

## Test the Application

Test the database connection by performing the following operations:

| Test   | Action                         |
| ------ | ------------------------------ |
| Add    | Add a new contact              |
| Edit   | Modify an existing contact     |
| Delete | Remove a contact               |
| Verify | Confirm that changes are saved |

## Result

The web application successfully connects to the Amazon RDS MySQL database.

The application can:

* Add contacts.
* Edit contacts.
* Remove contacts.
* Retrieve stored information.
* Persist data in the RDS database.

The RDS Multi-AZ deployment provides a standby database in another Availability Zone.

## Architecture

```text
Web Browser
     |
     v
Web Server / EC2
     |
     | MySQL Port 3306
     v
DB Security Group
     |
     v
Amazon RDS MySQL
     |
     +----------------------+
     |                      |
     v                      v
Availability Zone 1    Availability Zone 2
Primary Database       Standby Database
```

## Security

The RDS database is not publicly accessible.

The DB Security Group allows MySQL traffic on port `3306` from the Web Security Group.

## Screenshot


## Conclusions
- I launch an Amazon RDS DB instance with high availability.
- I configure the DB instance to permit connections from my web server.
- I open a web application and interact with my database.
