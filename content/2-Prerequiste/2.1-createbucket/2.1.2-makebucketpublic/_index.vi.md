---
title : "Làm Bucket Public"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.1.2 </b> "
---

#### Cấu hình bucket

1. Click Bucket bạn vừa tạo.
  + Click **Properties**.

![S3](/images/2.prerequisite/004-createbucket.png)

2. Tại trang **Properties**.
  + Tại mục **Static website hosting** click chọn **Edit**.

![S3](/images/2.prerequisite/005-createbucket.png)

  + Chuyển **Static website hosting** sang chế độ **Enable**
  + Điền vào ô index document là ``index.html``
  + Click **Save changes**

![S3](/images/2.prerequisite/006-createbucket.png)

3. Tại trang **Permissions**.
  + Tại mục **Bucket policy** chọn **Edit**.

![S3](/images/2.prerequisite/007-createbucket.png)

  + Ta dán đoạn này vào : 
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::{tên-bucket}/*"
        }
    ]
}
  ```
{{% notice warning %}}
  Thay tên bucket vào {tên-bucket}
{{% /notice %}}
  + Click **Save Changes**
  
![S3](/images/2.prerequisite/008-createbucket.png)