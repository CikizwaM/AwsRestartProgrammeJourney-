# 🚀 AWS EC2 Auto Scaling Lab

## 📌 Overview

In this lab, I created and tested an **AWS EC2 Auto Scaling environment** for a web application.

I used the following AWS services:

* Amazon EC2
* Amazon Machine Image (AMI)
* Application Load Balancer
* Target Groups
* Launch Templates
* Auto Scaling Groups
* Amazon CloudWatch
* AWS CLI

The objective was to create a web server, create a custom AMI from the server, and then use **Auto Scaling** to automatically launch additional EC2 instances when CPU utilization increased.

---

# 🏗️ Lab Architecture

The environment included:


```

---

# 🟢 Task 1: Creating a New AMI for Amazon EC2 Auto Scaling

## Task 1.1: Connecting to the Command Host

I opened the **EC2 Management Console** and selected **Instances**.

I selected the **Command Host** instance and clicked **Connect**.

I used **EC2 Instance Connect** to access the Command Host.

```
---

## Task 1.2: Configuring the AWS CLI

I checked the AWS Region using the following command:

```bash
curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region
```

The lab was running in:

```text
us-west-2
```

I then configured the AWS CLI:

```bash
aws configure
```

I entered:

```text
AWS Access Key ID: Press Enter
AWS Secret Access Key: Press Enter
Default region name: us-west-2
Default output format: json
```

I then navigated to the lab directory:

```bash
cd /home/ec2-user/
```

### 📸 Screenshot

```markdown   ,     #_
   ~\_  ####_        Amazon Linux 2
  ~~  \_#####\
  ~~     \###|       AL2 End of Life is 2026-06-30.
  ~~       \#/ ___
   ~~       V~' '->
    ~~~         /    A newer version of Amazon Linux is available!
      ~~._.   _/
         _/ _/       Amazon Linux 2023, GA and supported until 2029-06-30.
       _/m/'           https://aws.amazon.com/linux/amazon-linux-2023/

[ec2-user@ip-10-0-1-252 ~]$ curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   475  100   475    0     0   176k      0 --:--:-- --:--:-- --:--:--  231k
  "region" : "us-west-2",
[ec2-user@ip-10-0-1-252 ~]$ aws configure
AWS Access Key ID [None]: ASIARLVFCW6PRF6B5BZY
AWS Secret Access Key [None]: gnXauagIlNfBYVcAp1hGyjACymuHm2p181ZpcYMT
Default region name [us-west-2]: 
Default output format [None]: json
```
---

## Task 1.3: Creating a New EC2 Instance

I inspected the `UserData.txt` file provided by the lab:

```bash
more UserData.txt
```

The script was used to configure the web server and install the web application.

I copied the following values from the lab **Details** section:

* `KEYNAME`
* `AMIID`
* `HTTPACCESS`
* `SUBNETID`

I then used these values in the `aws ec2 run-instances` command:

```bash
aws ec2 run-instances \
--key-name KEYNAME \
--instance-type t3.micro \
--image-id AMIID \
--user-data file:///home/ec2-user/UserData.txt \
--security-group-ids HTTPACCESS \
--subnet-id SUBNETID \
--associate-public-ip-address \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]' \
--output text \
--query 'Instances[*].InstanceId'
```

The command returned the **Instance ID** of the new WebServer instance.

I saved the Instance ID because it was required for the following steps.

### 📸 Screenshot

```markdown
[ec2-user@ip-10-0-1-252 ~]$ cd /home/ec2-user/
[ec2-user@ip-10-0-1-252 ~]$ more UserData.txt
#!/bin/bash
yum update -y --security
amazon-linux-extras install epel -y
yum -y install httpd php stress
systemctl enable httpd.service
systemctl start httpd
cd /var/www/html
wget http://aws-tc-largeobjects.s3.amazonaws.com/CUR-TF-100-TULABS-1/10-lab-autoscaling-linux/s3/ec2-stress.zip
unzip ec2-stress.zip

echo 'UserData has been successfully executed. ' >> /home/ec2-user/result
find -wholename /root/.*history -wholename /home/*/.*history -exec rm -f {} \;
find / -name 'authorized_keys' -exec rm -f {} \;
rm -rf /var/lib/cloud/data/scripts/*
[ec2-user@ip-10-0-1-252 ~]$ 
```

---

## Waiting for the Instance

I waited for the EC2 instance to reach the running state:

```bash
aws ec2 wait instance-running --instance-ids NEW-INSTANCE-ID
```

I replaced `NEW-INSTANCE-ID` with the Instance ID returned from the previous command.

---

## Getting the Public DNS Name

I used the following command to obtain the public DNS name:

```bash
aws ec2 describe-instances \
--instance-id NEW-INSTANCE-ID \
--query 'Reservations[0].Instances[0].NetworkInterfaces[0].Association.PublicDnsName'
```

I copied the DNS name and opened it in a new browser tab.

After waiting for the web server to finish installing, the web application was displayed successfully.

### 📸 Screenshot

```markdown
![Web Server Application](images/task1-webserver.png)
```

---

# Task 1.4: Creating a Custom AMI

After confirming that the web server was working, I created a custom AMI from the EC2 instance.

```bash
aws ec2 create-image \
--name WebServerAMI \
--instance-id NEW-INSTANCE-ID
```

The AMI was created with the name:

```text
WebServerAMI
```

This AMI was later used as the base image for the Auto Scaling instances.

### 📸 Screenshot

```markdown
![WebServer AMI](images/task1-webserver-ami.png)
```

---

# 🔵 Task 2: Creating an Auto Scaling Environment

In this task, I created the infrastructure required to automatically distribute traffic and scale the web application.

The main components were:

* Application Load Balancer
* Target Group
* Launch Template
* Auto Scaling Group

---

# Task 2.1: Creating an Application Load Balancer

I opened:

**EC2 → Load Balancers → Create Load Balancer**

I selected:

**Application Load Balancer**

I configured the load balancer with the following settings:

| Setting             | Value             |
| ------------------- | ----------------- |
| Load Balancer Name  | `WebServerELB`    |
| VPC                 | `Lab VPC`         |
| Availability Zone 1 | `Public Subnet 1` |
| Availability Zone 2 | `Public Subnet 2` |
| Security Group      | `HTTPAccess`      |

I removed the default security group and selected the `HTTPAccess` security group.

---

## Creating the Target Group

I created a target group with the following configuration:

| Setting           | Value           |
| ----------------- | --------------- |
| Target Type       | Instances       |
| Target Group Name | `webserver-app` |
| Health Check Path | `/index.php`    |

I configured the load balancer to forward HTTP traffic to:

```text
webserver-app
```

The Application Load Balancer was created successfully with the name:

```text
WebServerELB
```

I copied the load balancer DNS name because I needed it later to test the application.

### 📸 Screenshot

```markdown
![Application Load Balancer](images/task2-load-balancer.png)
```

---

# Task 2.2: Creating a Launch Template

I opened:

**EC2 → Launch Templates → Create Launch Template**

I configured the launch template as follows:

| Setting              | Value                                |
| -------------------- | ------------------------------------ |
| Launch Template Name | `web-app-launch-template`            |
| Description          | `A web server for the load test app` |
| AMI                  | `WebServerAMI`                       |
| Instance Type        | `t3.micro`                           |
| Key Pair             | Don't include in launch template     |
| Security Group       | `HTTPAccess`                         |

I then created the launch template successfully.

### 📸 Screenshot

```markdown
![Launch Template](images/task2-launch-template.png)
```

---

# Task 2.3: Creating an Auto Scaling Group

I selected:

**web-app-launch-template → Actions → Create Auto Scaling Group**

I created the Auto Scaling Group with the name:

```text
Web App Auto Scaling Group
```

## Network Configuration

I configured:

| Setting       | Value                  |
| ------------- | ---------------------- |
| VPC           | `Lab VPC`              |
| Subnet 1      | `Private Subnet 1`     |
| Subnet 2      | `Private Subnet 2`     |
| Target Group  | `webserver-app`        |
| Health Checks | Elastic Load Balancing |

---

## Auto Scaling Capacity

I configured the following values:

| Setting          | Value |
| ---------------- | ----: |
| Desired Capacity |     2 |
| Minimum Capacity |     2 |
| Maximum Capacity |     4 |

---

## Scaling Policy

I selected:

**Target Tracking Scaling Policy**

I configured:

```text
Metric Type: Average CPU utilization
Target Value: 50%
```

This configuration allows Auto Scaling to add or remove EC2 instances to maintain average CPU utilization close to 50%.

---

## Tags

I added the following tag:

```text
Key: Name
Value: WebApp
```

I then created the Auto Scaling Group.

### 📸 Screenshot

```markdown
![Auto Scaling Group](images/task2-auto-scaling-group.png)
```

---

# 🟡 Task 3: Verifying the Auto Scaling Configuration

After creating the Auto Scaling Group, I opened the **EC2 Instances** page.

Two `WebApp` instances were automatically launched because the desired capacity was set to `2`.

Initially, the instances were still initializing.

I waited until the status checks showed:

```text
2/2 checks passed
```

### 📸 Screenshot

```markdown
![WebApp Instances](images/task3-instances.png)
```

---

## Checking Target Group Health

I opened:

**EC2 → Target Groups → webserver-app**

I selected the **Targets** tab.

The two Auto Scaling instances appeared in the target group.

I refreshed the page until both instances showed:

```text
healthy
```

This confirmed that the Application Load Balancer could successfully communicate with the EC2 instances.

### 📸 Screenshot

```markdown
![Healthy Targets](images/task3-healthy-targets.png)
```

---

# 🔴 Task 4: Testing the Auto Scaling Configuration

I opened the **DNS name of the Application Load Balancer** in a new browser tab.

The web application loaded successfully through the load balancer.

### 📸 Screenshot

```markdown
![Load Balancer Test](images/task4-load-balancer-test.png)
```

---

## Starting the Stress Test

On the web application, I selected:

```text
Start Stress
```

This started the CPU stress process on the EC2 instance handling the request.

The CPU utilization increased significantly.

### 📸 Screenshot

```markdown
![Start Stress](images/task4-start-stress.png)
```

---

## Monitoring Auto Scaling

I returned to:

**EC2 → Auto Scaling Groups → Web App Auto Scaling Group**

I selected the **Activity** tab.

After a few minutes, the Auto Scaling Group launched an additional EC2 instance.

This occurred because CloudWatch detected that the average CPU utilization had exceeded the configured target of **50%**.

The Auto Scaling policy responded by increasing the number of instances.

### 📸 Screenshot

```markdown
![Auto Scaling Activity](images/task4-scaling-activity.png)
```

---

# 📊 Final Configuration

| Component          | Configuration                       |
| ------------------ | ----------------------------------- |
| AMI                | `WebServerAMI`                      |
| Load Balancer      | `WebServerELB`                      |
| Target Group       | `webserver-app`                     |
| Launch Template    | `web-app-launch-template`           |
| Auto Scaling Group | `Web App Auto Scaling Group`        |
| Desired Capacity   | 2                                   |
| Minimum Capacity   | 2                                   |
| Maximum Capacity   | 4                                   |
| Scaling Metric     | Average CPU Utilization             |
| CPU Target         | 50%                                 |
| Public Subnets     | Public Subnet 1 & Public Subnet 2   |
| Private Subnets    | Private Subnet 1 & Private Subnet 2 |
| Security Group     | `HTTPAccess`                        |

---

# 🧠 What I Learned

During this lab, I learned how to:

* Create an EC2 instance using the AWS CLI.
* Configure the AWS CLI.
* Use EC2 Instance Connect.
* Create a custom Amazon Machine Image.
* Create an Application Load Balancer.
* Configure a Target Group.
* Create an EC2 Launch Template.
* Create an Auto Scaling Group.
* Configure target tracking scaling.
* Use CloudWatch CPU utilization for Auto Scaling.
* Test automatic scaling by generating CPU load.
* Verify the health of EC2 instances.
* Monitor Auto Scaling activity.

---

# ✅ Conclusion

I successfully created and tested an **AWS EC2 Auto Scaling environment**.

The environment started with **2 EC2 instances** and was configured to scale between **2 and 4 instances**.

After starting the CPU stress test, CPU utilization increased above the **50% target**. The Auto Scaling Group detected the increased load and automatically launched an additional EC2 instance.

This demonstrated how AWS Auto Scaling can automatically increase compute capacity when application demand increases.

---
