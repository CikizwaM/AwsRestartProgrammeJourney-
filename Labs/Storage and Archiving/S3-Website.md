# Creating a Website on S3

In this lab I used the AWS Command Line Interface (AWS CLI) commands from an Amazon Elastic Compute Cloud (Amazon EC2) instance to:
- Create an Amazon Simple Storage Service (Amazon S3) bucket.
- Create a new AWS Identity and Access Management (IAM) user that has full access to the Amazon S3 service.
- Upload files to Amazon S3 to host a simple website for the Café & Bakery.
- Create a batch file that can be used to update the static website when you change any of the website files locally.

![S3 Website Architecture](./images/s3-website-overview.png)

## Task 1: Connect to an Amazon Linux EC2 instance using SSM
From the lab, I opened the **InstanceSessionUrl** and typed:
```bash
sh-4.2$ sudo su -l ec2-user
[ec2-user@ip-10-200-0-238 ~]$ pwd
/home/ec2-user
```

## Task 2: Configure the AWS CLI
Amazon Linux has already **AWS CLI** pre-installed. I run the command `aws configure` to update the AWS CLI software with credentials.
```bash
[ec2-user@ip-10-200-0-238 ~]$ aws configure
AWS Access Key ID [None]: <from lab details>
AWS Secret Access Key [None]: <from lab details>
Default region name [None]: us-west-2
Default output format [None]: json


## Task 3: Create an S3 bucket using the AWS CLI
The `s3api` command creates a new **S3 bucket** with the AWS credentials in this lab. By default, the S3 bucket is created in the us-east-1 Region.
Bucket must have a unique name, here I used `ciki182`.
```bash
[ec2-user@ip-10-200-0-238 ~]$ aws s3api create-bucket --bucket ciki182 --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2
{
    "Location": "http://ciki182.s3.amazonaws.com/"
}
```


