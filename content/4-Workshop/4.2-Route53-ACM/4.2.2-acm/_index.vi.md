---
title: "Yêu cầu và xác thực chứng chỉ ACM"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.2.2. </b> "
---

1. Mở **AWS Certificate Manager (ACM)** và chọn yêu cầu chứng chỉ public.

![ACM bước 1](acm.1.jpg)

2. Nhập các domain cần được bảo vệ bởi chứng chỉ.

![ACM bước 2](acm.2.jpg)

3. Chọn **DNS validation** để có thể xác thực thông qua Route53.

![ACM bước 3](acm.3.jpg)

4. Rà soát thông tin yêu cầu và gửi request tạo chứng chỉ.

![ACM bước 4](acm.4.jpg)

5. Mở các bản ghi xác thực được tạo ra và tạo hoặc xác nhận CNAME trong Route53.

![ACM bước 5](acm.5.jpg)

6. Chờ đến khi quá trình xác thực hoàn tất và trạng thái chứng chỉ chuyển sang **Issued**.

![ACM bước 6](acm.6.jpg)

7. Kiểm tra lại thông tin chứng chỉ trước khi gắn cho load balancer hoặc CloudFront.

![ACM bước 7](acm.7.jpg)
