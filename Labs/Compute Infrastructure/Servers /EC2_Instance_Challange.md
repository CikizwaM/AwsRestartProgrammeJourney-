# AWS EC2 Web Server Challenge Lab

## 📌 Project Overview

This project demonstrates how to deploy a simple web application using an **Amazon Linux EC2 instance** on AWS.

The objective was to create a public-facing EC2 web server, configure the required networking and security resources, install Apache HTTP Server (`httpd`), and deploy a custom HTML webpage.

## 🛠️ AWS Services Used

* Amazon EC2
* Amazon VPC
* Internet Gateway
* Route Tables
* Security Groups
* Amazon Linux
* Apache HTTP Server (`httpd`)
* EC2 Instance Connect

## 🏗️ Architecture

```text
                    Internet
                       │
                       ▼
                Internet Gateway
                       │
                       ▼
              ┌─────────────────┐
              │      VPC        │
              │   10.0.0.0/16   │
              │                 │
              │  Public Subnet  │
              │   10.0.1.0/24   │
              │        │        │
              │        ▼        │
              │   EC2 Instance  │
              │  Amazon Linux   │
              │     + httpd     │
              │        │        │
              │        ▼        │
              │  projects.html  │
              └─────────────────┘
```

## 🚀 Project Tasks

### 1. Create the VPC

A new VPC was created with the following configuration:

* **VPC CIDR:** `10.0.0.0/16`
* **Subnet:** Public subnet
* **Subnet CIDR:** `10.0.1.0/24`

### 2. Configure Internet Connectivity

An Internet Gateway was created and attached to the VPC.

The subnet's route table was configured with:

```text
Destination: 0.0.0.0/0
Target: Internet Gateway
```

This allowed the EC2 instance to communicate with the internet.

### 3. Launch the EC2 Instance

The EC2 instance was configured using:

| Configuration    | Value                       |
| ---------------- | --------------------------- |
| Operating System | Amazon Linux                |
| Instance Type    | `t3.micro`                  |
| Public IPv4      | Enabled                     |
| Root Volume      | General Purpose SSD (`gp2`) |
| Subnet           | Public subnet               |
| Web Server       | Apache HTTP Server          |

### 4. Configure Security Group

The security group allowed the following inbound traffic:

| Protocol | Port | Purpose                    |
| -------- | ---: | -------------------------- |
| SSH      |   22 | EC2 Instance Connect / SSH |
| HTTP     |   80 | Web traffic                |

## 🔧 User Data

The following user data was used when launching the EC2 instance:

```bash
#!/bin/bash

yum update -y
yum install -y httpd

systemctl enable httpd
systemctl start httpd

chmod 777 /var/www/html
```

This script automatically:

1. Updates the system.
2. Installs Apache HTTP Server.
3. Enables Apache to start automatically.
4. Starts the Apache service.
5. Gives users write permission to the web-server document root.

## 🌐 Web Page

The deployed webpage was saved as:

```text
/var/www/html/projects.html
```

HTML used:

```html
<!DOCTYPE html>
<html>
<body>
<h1>Ciki's re/Start Project Work</h1>
<p>EC2 Instance Challenge Lab</p>
</body>
</html>
```

The webpage was accessed using the EC2 instance's public IPv4 address:

```text
http://PUBLIC-IP/projects.html
```

## 🔌 Connecting to the EC2 Instance

The instance was accessed using **EC2 Instance Connect** through the AWS Management Console.

Apache was verified with:

```bash
sudo systemctl status httpd
```

The expected status was:

```text
Active: active (running)
```

## 📸 Project Evidence

The following screenshots demonstrate successful completion of the lab:

### EC2 System Log

The EC2 system log shows that the web server was successfully installed and configured.

![EC2 System Log](screenshots/ec2-system-log.png)


### Web Application

The browser displays the deployed `projects.html` webpage.

![Web Application](screenshots/webpage-success.png)

## 📚 What I Learned

Through this project, I practiced:

* Creating and configuring an AWS VPC
* Creating public subnets
* Configuring Internet Gateways
* Configuring route tables
* Launching Amazon Linux EC2 instances
* Configuring EC2 security groups
* Using EC2 Instance Connect
* Installing and managing Apache HTTP Server
* Deploying a basic HTML webpage
* Accessing an application through a public IPv4 address
* Troubleshooting basic AWS networking and web-server configuration

## ✅ Project Status

**Completed**

The EC2 instance was successfully configured as a web server and the HTML application was successfully deployed and accessed through a web browser.

---

## 👤 Author

**Ciki**

AWS re/Start — EC2 Instance Challenge Lab
