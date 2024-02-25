---
title : "Tạo API Gateway"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

### Tạo API Gateway

Trong bước này chúng ta sẽ tiến hành tạo REST API trên API Gateway để kích hoạt Lambda event và đẩy request tới Lambda event
1. Truy cập vào [giao diện quản trị dịch vụ API Gateway](console.aws.amazon.com/apigateway/)
2. Kiếm mục **REST API** và chọn **Build**.  

![api](/images/2.prerequisite/001-createapi.png)

3. Phần **API name** điền **S3 Upload**, còn lại để mặc định.
  + Click *Create API*


![api](/images/2.prerequisite/002-createapi.png)

4. Tại mục Resources click **Create resource**.  

![api](/images/2.prerequisite/003-createapi.png)

5. Trong mục **Resource name**, điền **upload**
  + Click **Create resource**

![api](/images/2.prerequisite/004-createapi.png)

6. Click **Create method**.

![api](/images/2.prerequisite/005-createapi.png)

7. Tại trang **Create method**
  + Mục **Method type** để **POST**
  + Enable **Lambda proxy intergration**
  + Mục **Lambda function** chọn **s3-upload** mình vừa tạo
  + Click **Create method**

![api](/images/2.prerequisite/006-createapi.png)

8. Tại thanh bên trái chọn **API settings**
  + Tại mục *Binary media types** chọn **Managae mdeia types**

![api](/images/2.prerequisite/007-createapi.png)

9. Tại trang **Manage binary media types**
  + Chọn **Add binary media type** và thêm:
    + \*/\*
    + multipart/form-data
  + Click **Save changes**

![api](/images/2.prerequisite/008-createapi.png)

10. Tại thanh bên trái chọn **Resources** 
  + Chọn **Deploy API**

![api](/images/2.prerequisite/009-createapi.png)

11. Ở mục **Stage** chọn *\*New stage\**
  + Mục **Stage name** để *staging*
  + Click **Deploy**

![api](/images/2.prerequisite/010-createapi.png)

12. Sau đó bạn hãy copy lại Invoke URL và tới phần tiếp theo

![api](/images/2.prerequisite/011-createapi.png)