# S3 Static Website Hosting — Deployment Steps

## Overview

The Cloud Fitness Gym website was hosted on Amazon S3 using S3's built-in static website hosting feature. No EC2, no server, no database, simply just files served directly from a storage bucket.

## Bucket setup

![S3 bucket created](./Images/S3/1.%20S3%20Bucket%20Created.png)

## Steps Taken

### 1. index.html set as the index document

Static website hosting needs a designated "index document" which is the file returned when someone requests the bucket root URL. In this case, `index.html` is the expected name and matches what the site displays when accessed.

### 2. error.html created and set as the error document

Without a custom error document, S3 returns a raw, generic XML error page for any broken link or missing file. An `error.html` was built in the same style as `index.html` so a mistyped URL still looks professional and on-brand.

### 3. Block Public Access disabled

By default, every new S3 bucket blocks all public access, since most buckets store private data. A public website needs the opposite so that anyone should be able to read the files. This has to be explicitly configured.

![Block Public Access disabled](./Images/S3/3.%20Unblock%20Public%20Access.png)

### 4. Bucket policy added

Turning off "Block Public Access" alone isn't enough as S3 still denies access unless a policy explicitly allows it. The policy added grants `s3:GetObject` (read-only) to everyone (`Principal: "*"`) for all objects in the bucket.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cloudfitnessgym/*"
        }
    ]
}
```

### 5. Static website hosting enabled

The website endpoint (`bucket.s3-website-<region>.amazonaws.com`) behaves like a website (index document, error document, etc.). It only supports HTTP, not HTTPS which is a known S3 limitation.

**Live endpoint:** `http://cloudfitnessgym.s3-website.af-south-1.amazonaws.com`

![Static website hosting enabled](./Images/S3/2.%20Enable%20Static%20Hosting.png)

