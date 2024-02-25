---
title : "Create bucket"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1.1 </b> "
---

#### Create Bucket
1. Access the [S3 management console](https://s3.console.aws.amazon.com/s3/home?region=ap-southeast-1).
   - Click **Create Bucket**.

![S3](/images/2.prerequisite/001-createbucket.png)

2. On the **Create Bucket** page:
   - Under **AWS Region**, select **ap-south-east-1**.
   - Under **Bucket name**, enter **bucket-for-lambda** followed by your preferred **id** (I used bucket-for-lambda-55555).

![S3](/images/2.prerequisite/002-createbucket.png)

   - Under **Block Public Access settings for this bucket**, uncheck the box.
   - Leave the rest as default.
   - Click **Create Bucket**.

![S3](/images/2.prerequisite/003-createbucket.png)