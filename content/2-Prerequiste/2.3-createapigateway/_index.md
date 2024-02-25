---
title : "Create API Gateway"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

### Create API Gateway

In this step, we will proceed to create a REST API on API Gateway to trigger Lambda events and forward requests to Lambda events.

1. Access the [API Gateway service management interface](console.aws.amazon.com/apigateway/).
2. Look for **REST API** and select **Build**.

![api](/images/2.prerequisite/001-createapi.png)

3. Under **API name**, enter **S3 Upload**, leave the rest as default.
   - Click *Create API*.

![api](/images/2.prerequisite/002-createapi.png)

4. Click **Create resource** under the Resources section.

![api](/images/2.prerequisite/003-createapi.png)

5. In the **Resource name** field, enter **upload**.
   - Click **Create resource**.

![api](/images/2.prerequisite/004-createapi.png)

6. Click **Create method**.

![api](/images/2.prerequisite/005-createapi.png)

7. On the **Create method** page:
   - Set **Method type** to **POST**.
   - Enable **Lambda proxy integration**.
   - Under **Lambda function**, select the **s3-upload** function you created earlier.
   - Click **Create method**.

![api](/images/2.prerequisite/006-createapi.png)

8. From the left sidebar, select **API settings**.
   - Under **Binary media types**, choose **Manage media types**.

![api](/images/2.prerequisite/007-createapi.png)

9. On the **Manage binary media types** page:
   - Select **Add binary media type** and add:
     - \*/\*
     - multipart/form-data
   - Click **Save changes**.

![api](/images/2.prerequisite/008-createapi.png)

10. From the left sidebar, select **Resources**.
    - Choose **Deploy API**.

![api](/images/2.prerequisite/009-createapi.png)

11. Under **Stage**, select *\*New stage\**.
    - Set **Stage name** to *staging*.
    - Click **Deploy**.

![api](/images/2.prerequisite/010-createapi.png)

12. Copy the Invoke URL and proceed to the next step.

![api](/images/2.prerequisite/011-createapi.png)