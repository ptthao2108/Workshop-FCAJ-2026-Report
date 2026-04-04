---
title: "Triển khai Redis"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.5.2. </b> "
---

#### Tổng quan

Mục này trình bày các bước tạo **Redis** trên **ElastiCache** và cấu hình tích hợp với ứng dụng.

#### Các bước triển khai

1. Trước khi tạo cache, vào **ElastiCache > Subnet groups** để tạo subnet group cho Redis.

![Redis bước 1](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.1.jpg)

2. Tạo subnet group `lunchsync-redis-subnets`, chọn VPC `lunchsync-vpc` và thêm 2 private subnet ở 2 AZ khác nhau.

![Redis bước 2](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.2.jpg)

3. Kiểm tra lại danh sách subnet group để xác nhận `lunchsync-redis-subnets` đã được tạo.

![Redis bước 3](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.3.jpg)

4. Mở **Redis OSS caches** và bắt đầu tạo cluster mới.

![Redis bước 4](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.4.jpg)

5. Chọn engine **Redis OSS**, loại triển khai **Node-based cluster** và để **Cluster mode = Disabled**.

![Redis bước 5](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.5.jpg)

6. Đặt tên cluster là `lunchsync-redis`, chạy trong **AWS Cloud**, bật **Multi-AZ** và **Auto-failover**, giữ port `6379`.

![Redis bước 6](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.6.jpg)

7. Chọn node type `cache.t3.micro` và gắn cluster vào subnet group `lunchsync-redis-subnets`.

![Redis bước 7](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.7.jpg)

8. Đặt placement sao cho primary và replica nằm ở 2 AZ khác nhau để đảm bảo failover.

![Redis bước 8](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.8.jpg)

9. Ở phần security, bật **Encryption at rest**, bật **Encryption in transit = Required** và gắn security group `redis-sg`.

![Redis bước 9](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.9.jpg)

10. Cấu hình backup và maintenance: bật automatic backups, chọn retention, maintenance window và auto minor version upgrade.

![Redis bước 10](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.10.jpg)

11. Bật **Slow logs** và đưa log về CloudWatch Logs.

![Redis bước 11](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.11.jpg)

12. Bật tiếp **Engine logs** và cấu hình log group tương ứng trong CloudWatch.

![Redis bước 12](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.12.jpg)

13. Rà lại phần backup, maintenance và logging trước khi tạo cluster.

![Redis bước 13](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.13.jpg)

14. Sau khi cluster khả dụng, kiểm tra các thông tin chính như primary endpoint, reader endpoint, node type và trạng thái `Available`.

![Redis bước 14](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.14.jpg)

15. Nếu muốn siết bảo mật hơn, mở màn hình **Modify > Security** để kiểm tra lại TLS, AUTH token và security group của cluster.

![Redis bước 15](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.15.jpg)

16. Chuyển sang **AWS Secrets Manager**, tạo secret kiểu **Other type of secret** để lưu `auth_token`, `host` và `port` của Redis.

![Redis bước 16](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.16.jpg)

17. Đặt tên secret là `lunchsync/redis`.

![Redis bước 17](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.17.jpg)

18. Bỏ qua rotation nếu chưa cần, giữ secret ở dạng tĩnh rồi hoàn tất tạo secret.

![Redis bước 18](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.18.jpg)

19. Quay lại ElastiCache và kiểm tra danh sách node, primary endpoint và reader endpoint để sẵn sàng tích hợp vào backend.

![Redis bước 19](/images/4-Workshop/4.5-Database-Caching/4.5.2-redis/r.19.jpg)
