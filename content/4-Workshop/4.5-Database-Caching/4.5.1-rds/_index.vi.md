---
title: "Triển khai RDS"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.5.1. </b> "
---

#### Tổng quan

Mục này trình bày các bước tạo **RDS instance** và cấu hình kết nối cơ bản cho backend.

#### Các bước triển khai

1. Trước khi tạo database, vào **RDS > Subnet groups** để chuẩn bị DB subnet group cho private subnet.

![RDS bước 0.1](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.1.jpg)

2. Tạo subnet group `lunchsync-rds-subnet`, mô tả rõ dùng cho RDS của LunchSync và chọn VPC `lunchsync-vpc`.

![RDS bước 0.2](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.2.jpg)

3. Chọn 2 private subnet của `ap-southeast-1a` và `ap-southeast-1b` rồi thêm vào subnet group.

![RDS bước 0.3](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.3.jpg)

4. Kiểm tra lại danh sách subnet group để xác nhận `lunchsync-rds-subnet` đã ở trạng thái **Complete**.

![RDS bước 0.4](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.4.jpg)

5. Mở **Databases** và chọn **Create database** để bắt đầu tạo PostgreSQL instance.

![RDS bước 1](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.1.jpg)

6. Chọn **PostgreSQL**, dùng **Standard create / Full configuration**, template kiểu `Dev/Test` và triển khai `Single-AZ`.

![RDS bước 2](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.2.jpg)

![RDS bước 3](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.3.jpg)

7. Đặt `DB instance identifier = lunchsync-db`, master username là `lunchsync`, và chọn để **AWS Secrets Manager** tự quản lý credentials.

![RDS bước 4](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.4.jpg)

![RDS bước 5](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.5.jpg)

8. Chọn instance class `db.t3.micro`, storage `gp3`, sau đó cấu hình connectivity với `lunchsync-vpc`, subnet group `lunchsync-rds-subnet`, `Public access = No` và security group `rds-sg`.

![RDS bước 6](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.6.jpg)

![RDS bước 7](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.7.jpg)

9. Rà soát additional configuration, backup và maintenance rồi gửi yêu cầu tạo database.

![RDS bước 8](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.8.jpg)

10. Khi instance `lunchsync-db` chuyển sang **Available**, lưu lại endpoint `:5432`, ARN secret do RDS tạo và xác nhận database không public ra Internet.

![RDS bước 9](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.9.jpg)
