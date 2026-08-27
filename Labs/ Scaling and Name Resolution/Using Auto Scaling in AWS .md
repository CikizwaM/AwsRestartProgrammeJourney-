# Using Auto Scaling in AWS (Linux)
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

![Starting Architecture](./Images/Capture.PNG)

![Final Architecture](./Images/FinalArchitecture.png)

### 1.2 Creating a New EC2 Instance
I checked the AWS Region using the following command:

curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region

I checked the AWS Region using the following command:

curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region

The lab was running in:

us-west-2

I then configured the AWS CLI:

aws configure

I entered the following:

AWS Access Key ID: Press Enter
AWS Secret Access Key: Press Enter
Default region name: us-west-2
Default output format: json

I then moved to the lab directory:

cd /home/ec2-user/

```bash
   ,     #_
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





