---
title: "Cấu hình security group"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 4.3.4. </b> "
---

1. Mở phần **Security Groups** và rà soát các group được dùng cho tài nguyên của ứng dụng.

![SG bước 1](sg.1.jpg)

2. Cấu hình inbound rules cho security group phía ALB.

![SG bước 2](sg.2.jpg)

3. Cấu hình security group của backend để chỉ nhận lưu lượng từ ALB hoặc nguồn tin cậy.

![SG bước 3](sg.3.jpg)

4. Rà soát bộ rule cuối cùng và lưu cấu hình security group.

![SG bước 4](sg.4.jpg)
