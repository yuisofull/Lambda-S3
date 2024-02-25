---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Serverless
Serverless là mô hình phát triển ứng dụng trên cloud mà cho phép ta xây dựng và chạy applications của chúng ta một cách dễ dàng, mà không cần quản lý server gì hết.

Đây là 4 lợi ích mình thấy của mô hình Serverless:
- Giảm chi phí của việc quản lý và vận hành server, đây là công việc thường xuyên của DevOps, ta có thể giảm số lượng công việc mà DevOps cần phải làm. Như tạo, quản lý và monitor EC2.
- Tự động scale và high-availability: FaaS của chúng ta sẽ tự động scale theo traffic, ta không cần cấu hình gì nhiều, trừ khi ta muốn nó scale theo một cách cụ thế nào đó.
- Tối ưu tiền sử dụng cloud: đối với cloud vần đề quan trọng nhất là tiền hàng tháng, khi ta xài FaaS thì ta sẽ chỉ cần trả tiền cho từng function được trigger.
Hỗ trợ nhiều ngôn ngữ khác nhau: ta có thể dùng nhiều ngôn ngữ khác nhau để viết FaaS.

### AWS Lambda
Lambda là một dịch vụ serverless compute cho phép bạn chạy ứng dụng mà không cần khởi tạo hoặc quản lý máy chủ. Lambda chạy trên nển tảng hạ tầng có tính khả dụng cao và thực hiện tất cả việc quản lý tài nguyên tính toán, bao gồm bảo trì máy chủ và hệ điều hành, cung cấp dung lượng, tự động mở rộng quy mô và ghi log. Với Lambda, bạn có thể phát triển hầu hết mọi loại ứng dụng hoặc dịch vụ phụ trợ.

Bạn tổ chức ứng dụng của mình thành các Lambda function. Lambda function chỉ chạy khi cần thiết và có khả năng tự động mở rộng quy mô, từ một vài yêu cầu mỗi ngày đến hàng nghìn mỗi giây. Bạn chỉ phải trả cho thời gian tính toán mà bạn sử dụng — AWS sẽ không tính phí khi ứng dụng của bạn không chạy.

Trong bài thực hành này chúng ta sẽ giúp người dùng đănG file bucket với POST request sử dụng Gateway API và Lambda Function