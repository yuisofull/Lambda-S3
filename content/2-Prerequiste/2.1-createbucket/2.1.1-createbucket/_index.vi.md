---
title : "Tạo Bucket "
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1.1 </b> "
---


#### Tạo Bucket
1. Truy cập [giao diện quản trị dịch vụ S3](https://s3.console.aws.amazon.com/s3/home?region=ap-southeast-1)
  + Click **Create Bucket**.

![S3](/images/2.prerequisite/001-createbucket.png)

2. Tại trang **Create Bucket**.
  + Tại mục **AWS Region** chọn **ap-south-east-1**
  + Tại mục **Bucket name** điền **bucket-for-lambda** + **id** bạn muốn (mình để bucket-for-lambda-55555).

![S3](/images/2.prerequisite/002-createbucket.png)

  + Tại mục **Block Public Access settings for this bucket** các bạn uncheck
  + Các mục còn lại để mặc định.
  + Click **Create Bucket**.

![S3](/images/2.prerequisite/003-createbucket.png)