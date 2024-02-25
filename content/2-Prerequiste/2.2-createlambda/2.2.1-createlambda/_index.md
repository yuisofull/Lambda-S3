---
title : "Create Lambda Function "
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.2.1 </b> "
---

#### Create Lambda Function
1. Access the [Lambda service management interface](console.aws.amazon.com/lambda/home)
   - Click **Create a function**.

![Lambda](/images/2.prerequisite/001-createlambda.png)

2. On the **Create function** page:
   - Under **Function name**, enter **s3-upload**.
   - Under **Runtime**, select **Amazon Linux 2023**.
   - Under **Architecture**, select **arm64**.
   - Click **Create Function**.

![Lambda](/images/2.prerequisite/002-createlambda.png)

3. After successful creation, select **Configuration**.
   - Click on **role**.

![Lambda](/images/2.prerequisite/003-createlambda.png)

4. On the **IAM** page:
   - Select **Add permissions**.
   - Select **Attach policies**.

![Lambda](/images/2.prerequisite/004-createlambda.png)

   - For convenience, search and select **AmazonS3FullAccess**. In production, we would configure more granular permissions.
   - Click **Add permissions**.

![Lambda](/images/2.prerequisite/005-createlambda.png)