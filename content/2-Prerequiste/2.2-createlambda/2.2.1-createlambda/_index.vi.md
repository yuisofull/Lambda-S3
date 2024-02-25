---
title : "Tạo Lambda Function"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.2.1 </b> "
---


#### Tạo Lambda Function
1. Truy cập [giao diện quản trị dịch vụ Lambda](console.aws.amazon.com/lambda/home)
  + Click **Create a function**.

![Lambda](/images/2.prerequisite/001-createlambda.png)

2. Tại trang **Create function**.
  + Tại mục **Function name** điền **s3-upload**
  + Tại mục **Runtime** điền **bucket-for-lambda** chọn **Amazon Linux 2023**
  + Tại mục **Architecture** chọn **arm64**
  + Click **Create Function**

![Lambda](/images/2.prerequisite/002-createlambda.png)

3. Sau khi tạo thành công, chọn mục **Configuration**
  + Click vào **role**

![Lambda](/images/2.prerequisite/003-createlambda.png)

4. Tại trang **IAM**
  + Chọn **Add permissions**
  + Chọn **Attach policies**

![Lambda](/images/2.prerequisite/004-createlambda.png)

  + Để thuận tiện, kiếm và chọn **AmazonS3FullAccess**. Trong production chúng ta sẽ cấu hình quyền chi tiết hơn
  + Click **Add permissions**

![Lambda](/images/2.prerequisite/005-createlambda.png)