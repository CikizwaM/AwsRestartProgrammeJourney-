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

## Task 4: Create a new IAM user that has full access to Amazon S3
The AWS CLI command `aws iam create-user` creates a new IAM user for your AWS account. The option `--user-name` is used to create the name of the user and must be unique within the account.
```bash
[ec2-user@ip-10-200-0-238 ~]$ aws iam create-user --user-name awsS3user
{
   "User": {
"UserName": "awsS3user",
        "Path": "/",
        "CreateDate": "2026-07-12T16:52:10Z",
        "UserId": "AIDAWFDSMH5DJCRZ2M4FO",
        "Arn": "arn:aws:iam::423294156614:user/awsS3user"
 }
}
```
Then the AWS CLI command `aws iam create-login-profile` creates a login profile for the new user.
```bash
[ec2-user@ip-10-200-0-238 ~]$ aws iam create-login-profile --user-name awsS3user --password Training123!
{
"LoginProfile": {
        "UserName": "awsS3user",
        "CreateDate": "2026-07-12T16:53:35Z",
        "PasswordResetRequired": false
  }
}
```
I login in with the new IAM user credentials:
- Account ID: ID of the account VocLabsUser from the AWS Management Console
- IAM user name: awsS3user
- password: Training123!

The IAM user didn't have access to the S3 bucket. Using the terminal I serach policy that grants full access to Amazon S3.
```bash
sh-4.2$ aws iam list-policies --query "Policies[?contains(PolicyName,'S3')]"
[
 {
"PolicyName": "AmazonS3FullAccess",
        "PermissionsBoundaryUsageCount": 0,
        "CreateDate": "2015-02-06T18:40:58Z",
        "AttachmentCount": 0,
        "IsAttachable": true,
        "PolicyId": "ANPAIFIR6V6BVTRAHWINE",
        "DefaultVersionId": "v2",
        "Path": "/",
        "Arn": "arn:aws:iam::aws:policy/AmazonS3FullAccess",
        "UpdateDate": "2021-09-27T20:16:37Z"
  },
...
```













































