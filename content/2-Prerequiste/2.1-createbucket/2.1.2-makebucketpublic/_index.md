---
title : "Make Bucket Public"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.1.2 </b> "
---

#### Configure bucket

1. Click on the bucket you just created.
   - Click **Properties**.

![S3](/images/2.prerequisite/004-createbucket.png)

2. On the **Properties** page:
   - Under **Static website hosting**, click **Edit**.

![S3](/images/2.prerequisite/005-createbucket.png)

   - Switch **Static website hosting** to **Enable**.
   - Enter ``index.html`` in the index document box.
   - Click **Save changes**.

![S3](/images/2.prerequisite/006-createbucket.png)

3. On the **Permissions** page:
   - Under **Bucket policy**, click **Edit**.

![S3](/images/2.prerequisite/007-createbucket.png)

   - Paste the following policy:
   ```json
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::{bucket-name}/*"
        }
    ]
}
  ```
{{% notice warning %}}
  Change {bucket-name} to your bucket name
{{% /notice %}}
   - Click **Save Changes**.

![S3](/images/2.prerequisite/008-createbucket.png)