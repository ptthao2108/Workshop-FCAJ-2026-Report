---
title: "Tạo target group"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.3.2. </b> "
---

1. Mở khu vực **EC2 / Target Groups** và bắt đầu tạo target group mới.

![TG bước 1](tg.1.jpg)

2. Chọn kiểu target và giao thức phù hợp với backend service.

![TG bước 2](tg.2.jpg)

3. Nhập tên target group và bảo đảm chọn đúng VPC của ứng dụng.

![TG bước 3](tg.3.jpg)

4. Cấu hình health check để load balancer có thể kiểm tra trạng thái backend.

![TG bước 4](tg.4.jpg)

5. Đăng ký các instance hoặc target sẽ nhận lưu lượng truy cập.

![TG bước 5](tg.5.jpg)

6. Rà soát lại cấu hình target group và hoàn tất bước tạo.

![TG bước 6](tg.6.jpg)
