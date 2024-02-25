---
title : "Upload file"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

Trong bước này, chúng ta sẽ thực hiện đăng hình ảnh lên S3 sử dụng **curl** hoặc **Postman**

#### Curl

Các bạn mở terminal và nhập:
```
curl -v --request POST '{Invoke URL ở phần trước}/upload' --form file='@{Đường dẫn tới hình ảnh}'
```
Ví dụ trường hợp của tôi sẽ là:
```
curl -v --request POST 'https://4x87mid1b8.execute-api.ap-southeast-1.amazonaws.com/staging/upload' --form file='@C:\Users\Admin\ok.png'
```
Nêu thành công sẽ trả về link ở cuối như thế này:
```
{"link":"https://lambda-test-12333.s3.ap-southeast-1.amazonaws.com/ok-1708837492785249474.png"}* Connection #0 to host 4x87mid1b8.execute-api.ap-southeast-1.amazonaws.com left intact
```
#### Postman

+ Phần **body** chọn **form-data**
    + Thêm **key** là **file**
        + Phần value chọn **file** bạn muốn

![upload](/images/3.upload/001-upload.png)

Sau khi thành công, các bạn có thể thấy file mình vừa upload trên bucket

![upload](/images/3.upload/002-upload.png)