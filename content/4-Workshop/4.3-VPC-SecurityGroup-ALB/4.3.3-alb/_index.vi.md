---
title: "Tạo Application Load Balancer"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 4.3.3. </b> "
---

1. Mở phần **Load Balancers** và chọn tạo **Application Load Balancer**.

![ALB bước 1](alb.1.jpg)

2. Nhập các thông tin cơ bản như tên, scheme và kiểu địa chỉ IP.

![ALB bước 2](alb.2.jpg)

3. Chọn VPC và các subnet sẽ triển khai ALB.

![ALB bước 3](alb.3.jpg)

4. Gắn security group để kiểm soát lưu lượng đi vào ALB.

![ALB bước 4](alb.4.jpg)

5. Cấu hình listener và chuẩn bị rule chuyển tiếp.

![ALB bước 5](alb.5.jpg)

6. Liên kết listener với target group đã tạo ở bước trước.

![ALB bước 6](alb.6.jpg)

7. Rà soát phần tóm tắt trước khi tạo load balancer.

![ALB bước 7](alb.7.jpg)

8. Kiểm tra listener và target group sau khi tạo xong.

![ALB bước 8](alb.8.jpg)

9. Xem lại thông tin chi tiết và DNS name của ALB.

![ALB bước 9](alb.9.jpg)

10. Kiểm tra health và phần cấu hình điều phối lưu lượng trên console.

![ALB bước 10](alb.10.jpg)

11. Xác nhận trạng thái cuối cùng của các tài nguyên ALB.

![ALB bước 11](alb.11.jpg)

12. Lưu lại trạng thái hoàn tất của ALB để dùng cho bước gắn DNS và kiểm thử sau đó.

![ALB bước 12](alb.12.jpg)
