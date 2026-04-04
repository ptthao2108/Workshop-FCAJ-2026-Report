---
title : "Giới thiệu"
date : 2026-03-20 
weight : 1
chapter : false
pre : " <b> 4.1. </b> "
---

#### Tổng quan

Workshop này mô tả quy trình triển khai hệ thống **LunchSync** lên AWS theo kiến trúc thực tế của dự án. Hệ thống được xây dựng theo mô hình nhiều lớp, bao gồm lớp định tuyến domain, chứng chỉ bảo mật, mạng nội bộ, cân bằng tải, xác thực người dùng, phân phối nội dung tĩnh, cơ sở dữ liệu quan hệ và bộ nhớ đệm để phục vụ ứng dụng ở môi trường cloud.

#### Các thành phần hệ thống

Hệ thống LunchSync được triển khai trên AWS với các thành phần chính:

- **Route53 và ACM** để quản lý DNS và cấp phát chứng chỉ SSL/TLS cho domain ứng dụng
- **VPC, Security Group, Target Group và ALB** để xây dựng lớp mạng, kiểm soát truy cập và điều phối lưu lượng đến backend
- **Amazon Cognito** để xử lý xác thực người dùng và hỗ trợ luồng đăng nhập an toàn
- **Amazon S3 và CloudFront** để lưu trữ, phân phối frontend tĩnh và tối ưu tốc độ truy cập
- **Amazon RDS** để cung cấp cơ sở dữ liệu quan hệ cho hệ thống
- **Redis** để bổ sung lớp cache, giảm tải truy vấn và cải thiện hiệu năng phản hồi

#### Workshop Architecture

![Workshop architecture](/images/proposal/lunchsync.png)

#### Tổng quan về Workshop

Trong workshop này, bạn sẽ triển khai một hệ thống nhiều giai đoạn để đưa ứng dụng LunchSync lên AWS theo từng lớp hạ tầng và dịch vụ. Bao gồm:

- **Thiết lập domain và bảo mật truy cập**: Cấu hình Route53 hosted zone và yêu cầu chứng chỉ ACM để chuẩn bị endpoint truy cập an toàn cho hệ thống
- **Thiết lập nền tảng mạng**: Tạo VPC, target group, Application Load Balancer và security group để xây dựng đường đi mạng giữa người dùng và backend
- **Thiết lập xác thực**: Cấu hình Cognito user pool, app client và hosted UI để hỗ trợ đăng nhập và quản lý người dùng
- **Thiết lập phân phối frontend**: Chuẩn bị S3 bucket, Origin Access Control và CloudFront distribution để triển khai frontend tĩnh một cách bảo mật và hiệu quả
- **Thiết lập tầng dữ liệu**: Triển khai RDS để lưu trữ dữ liệu nghiệp vụ và Redis để tăng tốc truy xuất bằng cơ chế cache

Sau khi hoàn thành workshop, bạn sẽ có một môi trường LunchSync trên cloud với đầy đủ các thành phần cần thiết để phục vụ truy cập, xác thực, lưu trữ dữ liệu và tối ưu hiệu năng.
