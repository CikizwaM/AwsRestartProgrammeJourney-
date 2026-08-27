AWS EC2 Auto Scaling Lab
Overview

In this lab, I created a scalable web application environment using Amazon EC2, Amazon Machine Images (AMI), Application Load Balancer, Launch Templates, Auto Scaling Groups, and Amazon CloudWatch.

The main objective was to create a web server, use it to create a custom AMI, and then configure an Auto Scaling environment that automatically launches additional EC2 instances when CPU utilization increases.

Architecture

The environment I created included:

Command Host EC2 instance – Used to run AWS CLI commands.
WebServer EC2 instance – Used as the source for the custom AMI.
WebServerAMI – Custom AMI created from the WebServer instance.
Application Load Balancer – Distributed traffic across the Auto Scaling instances.
Target Group – Registered the web application instances with the load balancer.
Launch Template – Defined how new instances should be launched.
Auto Scaling Group – Maintained between 2 and 4 instances.
CloudWatch – Monitored CPU utilization and triggered scaling.
