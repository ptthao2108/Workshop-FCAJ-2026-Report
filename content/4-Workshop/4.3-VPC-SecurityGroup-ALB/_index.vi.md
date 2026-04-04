---
title: "VPC, Security Group và ALB"
date: 2026-04-04
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

#### Tổng quan

Mục này trình bày các bước thiết lập **VPC**, **Target Group**, **ALB** và **Security Group** cho lớp mạng của hệ thống.

#### Các bước triển khai

1. Mở **VPC console**, chọn **Create VPC** và dùng mẫu **VPC and more** để AWS sinh sẵn subnet, route table và internet gateway.

![VPC bước 1](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/vpc.1.jpg)

1. Đặt tên VPC là `lunchsync-vpc`, dùng CIDR `10.0.0.0/16`, tạo 2 AZ, 2 public subnet và 2 private subnet.

![VPC bước 2](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/vpc.2.jpg)

1. Đặt **NAT gateways** là `None`, bật **S3 Gateway Endpoint** và giữ **Enable DNS hostnames / DNS resolution** để private subnet vẫn truy cập được S3.

![VPC bước 3](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/vpc.3.jpg)

4. Tạo VPC và kiểm tra workflow đã sinh đủ `public/private subnets`, `internet gateway`, `route tables` và `S3 endpoint`.

![VPC bước 4](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/vpc.4.jpg)

5. Chuyển sang **EC2 > Target Groups** và bắt đầu tạo target group cho backend.

![Target Group bước 1](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/tg.1.jpg)

6. Chọn **Target type = IP addresses**, đặt tên `lunchsync-tg-backend`, protocol `HTTP`, port `8080`.

![Target Group bước 2](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/tg.2.jpg)

7. Gắn target group vào `lunchsync-vpc`, giữ **Protocol version = HTTP1** và đặt health check path là `/health`.

![Target Group bước 3](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/tg.3.jpg)

1. Ở màn hình **Register targets**, lưu ý ECS sẽ tự đăng ký IP của task về sau, nên có thể tiếp tục mà chưa cần thêm IP thủ công.

![Target Group bước 4](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/tg.4.jpg)

9. Rà lại thông tin cuối cùng của target group: protocol `HTTP:8080`, health check `traffic-port`, path `/health`, success code `200`.

![Target Group bước 5](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/tg.5.jpg)

1.  Sau khi tạo xong, xác nhận `lunchsync-tg-backend` đã xuất hiện và hiện chưa có target nào được đăng ký.

![Target Group bước 6](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/tg.6.jpg)

11. Mở **EC2 > Load Balancers** và bắt đầu tạo load balancer mới.

![ALB bước 1](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.1.jpg)

12. Chọn loại **Application Load Balancer**.

![ALB bước 2](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.2.jpg)

13. Đặt tên ALB là `lunchsyn-alb`, chọn `Internet-facing` và `IPv4`.

![ALB bước 3](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.3.jpg)

14. Chọn VPC `lunchsync-vpc`, gắn ALB vào 2 public subnet `lunchsync-subnet-public1` và `lunchsync-subnet-public2`.

![ALB bước 4](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.4.jpg)

15. Tạo listener `HTTP:80` với action **Redirect to HTTPS**, status code `301`.

![ALB bước 5](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.5.jpg)

16. Tạo listener `HTTPS:443` và forward mặc định đến target group `lunchsync-tg-backend`.

![ALB bước 6](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.6.jpg)

17. Ở phần **Secure listener settings**, chọn certificate ACM cho `lunchsync.space` và giữ security policy được AWS khuyến nghị.

![ALB bước 7](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.7.jpg)

18. Rà soát thêm mục integration, đặc biệt nếu muốn giới hạn ALB chỉ nhận lưu lượng đi qua CloudFront ở các bước sau.

![ALB bước 8](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.8.jpg)

19. Kiểm tra trang summary: ALB dùng 2 public subnet, listener `80 -> 443`, listener `443 -> lunchsync-tg-backend`, rồi tạo load balancer.

![ALB bước 9](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.9.jpg)

20. Sau khi ALB được tạo, kiểm tra tab **Listeners and rules** để xác nhận `HTTP:80` đang redirect và `HTTPS:443` đang forward đúng target group.

![ALB bước 10](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.10.jpg)

21. Mở tab **Security** và kiểm tra security group đang gắn cho ALB, đồng thời lưu lại DNS name để dùng lại ở phần CloudFront.

![ALB bước 11](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.11.jpg)

22. Xác nhận security group của ALB hiện cho phép luồng đi từ CloudFront/prefix list tương ứng thay vì mở trực tiếp cho toàn Internet.

![ALB bước 12](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/alb.12.jpg)

23. Mở **Security Groups** để rà danh sách group trong VPC và chuẩn bị tạo group cho backend.

![Security Group bước 1](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/sg.1.jpg)

24. Tạo security group `backend-sg` trong `lunchsync-vpc`, cho phép inbound `Custom TCP 8080` từ security group của ALB/CloudFront và giữ outbound `All traffic`.

![Security Group bước 2](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/sg.2.jpg)

25. Kiểm tra security group `redis-sg` để bảo đảm chỉ mở `TCP 6379` từ `backend-sg`.

![Security Group bước 3](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/sg.3.jpg)

26. Kiểm tra security group `rds-sg` để bảo đảm chỉ mở `PostgreSQL 5432` từ `backend-sg`.

![Security Group bước 4](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/sg.4.jpg)

27. Ảnh tổng hợp cuối cùng dùng để rà nhanh toàn bộ security group của workshop: group cho ALB/CloudFront, `backend-sg`, `redis-sg`, `rds-sg` và các group mặc định trong VPC.

![Security Group tổng hợp](/images/4-Workshop/4.3-VPC-SecurityGroup-ALB/Screenshot%202026-04-03%20001755.jpg)
