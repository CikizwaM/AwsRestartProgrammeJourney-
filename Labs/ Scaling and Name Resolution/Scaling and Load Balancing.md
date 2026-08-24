AWS Auto Scaling and Load Balancing Lab
Overview

In this lab, I created an Amazon Machine Image (AMI) from an existing EC2 instance and used it to build an Auto Scaling environment. I created an Application Load Balancer, target group, launch template, and Auto Scaling group. I then tested load balancing and verified that Auto Scaling launched additional EC2 instances when CPU utilization increased.

Services Used
Amazon EC2
Amazon Machine Images (AMI)
Application Load Balancer (ALB)
Target Groups
EC2 Launch Templates
EC2 Auto Scaling
Amazon CloudWatch
Lab Architecture

The completed setup consisted of:

                    Internet
                       │
                       ▼
              ┌─────────────────┐
              │    LabELB       │
              │ Application     │
              │ Load Balancer   │
              └────────┬────────┘
                       │
                lab-target-group
                       │
          ┌────────────┴────────────┐
          │                         │
          ▼                         ▼
   ┌─────────────┐           ┌─────────────┐
   │ Lab Instance│           │ Lab Instance│
   │ Private     │           │ Private     │
   │ Subnet 1    │           │ Subnet 2    │
   └─────────────┘           └─────────────┘
          │                         │
          └────────────┬────────────┘
                       │
                 Auto Scaling
                       │
             ┌─────────┴─────────┐
             │                   │
        CPU > 50%          CPU decreases
             │                   │
             ▼                   ▼
       Add instances       Remove instances
Task 1: Creating an AMI for Auto Scaling

In this task, I created an AMI from the existing Web Server 1 EC2 instance. The AMI saves the contents of the boot disk so that identical instances can be launched later.

Steps
I opened the Amazon EC2 Management Console.
From the left navigation pane, I selected Instances.
I selected the running Web Server 1 instance.
From Actions, I selected:
Image and templates → Create image
I configured the image with the following values:
Setting	Value
Image name	Web Server AMI
Image description	Lab AMI for Web Server
I selected Create image.
AWS displayed the confirmation screen with the new AMI ID.
Screenshot

Screenshot: AMI successfully created

![AMI Created](./images/Task1-1.png)
Task 2: Creating a Load Balancer

In this task, I created an Application Load Balancer to distribute traffic across multiple EC2 instances and Availability Zones.

Load Balancer Configuration

I selected Create load balancer → Application Load Balancer.

I configured:

Setting	Value
Load balancer name	LabELB
VPC	Lab VPC
Availability Zone 1	Public Subnet 1
Availability Zone 2	Public Subnet 2
Security group	Web Security Group

I removed the default security group and selected the existing Web Security Group.

Screenshot

Screenshot: Application Load Balancer configuration

![Load Balancer Configuration](./images/Task2-1.png)
Creating the Target Group

I created a target group with the following configuration:

Setting	Value
Target type	Instances
Target group name	lab-target-group

I then created the target group and returned to the Load Balancer configuration.

Screenshot

Screenshot: Target group created

![Target Group](./images/Task2-2.png)

I selected lab-target-group as the default forwarding target and created the load balancer.

Screenshot

Screenshot: LabELB successfully created

![LabELB Created](./images/Task2-3.png)

I copied the DNS name of the load balancer and saved it for testing later in the lab.

Task 3: Creating a Launch Template

In this task, I created a launch template that the Auto Scaling group would use to launch EC2 instances. The template contains information such as the AMI, instance type, key pair, and security group.

I opened EC2 → Launch Templates and selected Create launch template.

Launch Template Configuration
Setting	Value
Launch template name	lab-app-launch-template
Template version description	A web server for the load test app
Auto Scaling guidance	Enabled
AMI	Web Server AMI
Instance type	t3.micro
Key pair	Don't include in launch template
Security group	Web Security Group

I then selected Create launch template.

AWS confirmed that:

Successfully created lab-app-launch-template
Screenshot

Screenshot: Launch template successfully created

![Launch Template](./images/Task3-1.png)
Task 4: Creating an Auto Scaling Group

I used the launch template to create an Auto Scaling group.

Auto Scaling Group

I selected:

lab-app-launch-template

and chose:

Actions → Create Auto Scaling group

For the Auto Scaling group name, I entered:

Lab Auto Scaling Group
Screenshot

Screenshot: Auto Scaling group creation

![Auto Scaling Group](./images/Task4-1.png)
Network Configuration

I selected:

Setting	Value
VPC	Lab VPC
Subnet 1	Private Subnet 1 (10.0.1.0/24)
Subnet 2	Private Subnet 2 (10.0.3.0/24)

Load Balancing Configuration

Under Configure advanced options, I selected:

Attach to an existing load balancer

Then:

Choose from your load balancer target groups

I selected:

lab-target-group | HTTP

For the health check type, I selected:

ELB

Screenshot

Screenshot: Auto Scaling load balancing configuration

![ASG Load Balancing](./images/Task4-2.png)
Group Size and Scaling Policy

I configured the group size as:

Setting	Value
Desired capacity	2
Minimum capacity	2
Maximum capacity	4

For the scaling policy, I selected:

Target tracking scaling policy

Then configured:

Setting	Value
Metric type	Average CPU utilization
Target value	50

This allows Auto Scaling to maintain average CPU utilization around 50% and automatically add or remove capacity as needed.

Screenshot

Screenshot: Auto Scaling group size and scaling policy

![Scaling Policy](./images/Task4-3.png)
Adding the Instance Tag

I added the following tag:

Key	Value
Name	Lab Instance

Then I selected Create Auto Scaling group.

Screenshot

Screenshot: Auto Scaling group successfully created

![ASG Created](./images/Task4-4.png)

The Auto Scaling group initially had zero instances while AWS launched instances to reach the desired capacity of two.

Task 5: Verifying Load Balancing

In this task, I verified that the Auto Scaling instances were running and registered with the target group.

I opened:

EC2 → Instances

I confirmed that two new instances named:

Lab Instance

were running.

Screenshot

Screenshot: Lab instances running

![Lab Instances](./images/Task5-1.png)
Checking Target Health

I opened:

Load Balancing → Target Groups

and selected:

lab-target-group

Under Registered targets, I confirmed that the two Lab Instance targets appeared.

I waited until both instances showed:

healthy

A healthy status indicates that the instances passed the load balancer health check and can receive traffic.

Screenshot

Screenshot: Healthy target instances

![Healthy Targets](./images/Task5-2.png)
Testing the Load Balancer

I opened a new browser tab and entered the LabELB DNS name that I copied earlier.

The Load Test application appeared.

This confirmed that the load balancer received the request and forwarded it to one of the EC2 instances.

Screenshot

Screenshot: Load Test application

![Load Test Application](./images/Task5-3.png)
Task 6: Testing Auto Scaling

In this task, I tested whether the Auto Scaling group could automatically launch additional instances when CPU utilization increased.

The Auto Scaling group was configured with:

Minimum: 2
Desired: 2
Maximum: 4
Checking CloudWatch Alarms

I opened:

CloudWatch → Alarms → All alarms

Two alarms created automatically by the Auto Scaling group were displayed.

I selected the alarm containing:

AlarmHigh

The initial state was:

OK

The alarm monitors CPU utilization above 50%.

Screenshot

Screenshot: CloudWatch AlarmHigh alarm

![CloudWatch Alarm](./images/Task6-1.png)
Generating Load

I returned to the Load Test application.

Next to the AWS logo, I selected:

Load Test

This caused the application to generate a high CPU load. The page automatically refreshed while the load was being generated.

Screenshot

Screenshot: Load Test generating CPU load

![Generating Load](./images/Task6-2.png)
Monitoring Auto Scaling

I returned to the CloudWatch console and refreshed the alarms periodically.

The AlarmHigh alarm eventually changed to:

In alarm

The CPU utilization increased above the 50% target.

Screenshot

Screenshot: AlarmHigh in alarm state

![Alarm In Alarm](./images/Task6-3.png)
Verifying Additional Instances

I returned to:

EC2 → Instances

I confirmed that more than two instances named:

Lab Instance

were now running.

This demonstrated that the Auto Scaling group automatically launched additional instances in response to the increased CPU load.

Screenshot

Screenshot: Additional instances launched

![Auto Scaling Instances](./images/Task6-4.png)
Task 7: Terminating Web Server 1

After creating the AMI and Auto Scaling environment, the original Web Server 1 instance was no longer required.

I selected:

Web Server 1

Then selected:

Instance state → Terminate instance

I confirmed the termination by selecting:

Terminate

Screenshot

Screenshot: Web Server 1 terminated

![Web Server Terminated](./images/Task7-1.png)
