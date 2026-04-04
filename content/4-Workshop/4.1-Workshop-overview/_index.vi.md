---
title: "Giới thiệu"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

#### Tổng quan

Mục này giới thiệu kiến trúc tổng thể và các thành phần AWS sử dụng trong quy trình triển khai hệ thống **LunchSync**.

#### Các thành phần hệ thống

Hệ thống LunchSync được triển khai trên AWS với các thành phần chính:

- **Route53 và ACM** để quản lý DNS và cấp phát chứng chỉ SSL/TLS cho domain ứng dụng
- **Amazon Cognito** để xử lý xác thực người dùng và hỗ trợ luồng đăng nhập an toàn
- **VPC, Security Group, Target Group và ALB** để xây dựng lớp mạng, kiểm soát truy cập và điều phối lưu lượng đến backend
- **Amazon ECR và AWS Fargate** để lưu trữ image, triển khai container backend và phục vụ quy trình CI/CD
- **Amazon RDS** để cung cấp cơ sở dữ liệu quan hệ cho hệ thống
- **Redis** để bổ sung lớp cache, giảm tải truy vấn và cải thiện hiệu năng phản hồi

#### Workshop Architecture

![Workshop architecture](/images/2-Proposal/lunchsync.png)


#### Tổng quan về Workshop

Trong workshop này, bạn sẽ triển khai một hệ thống nhiều giai đoạn để đưa ứng dụng LunchSync lên AWS theo từng lớp hạ tầng và dịch vụ. Bao gồm:

- **Thiết lập truy cập và xác thực**: Cấu hình Route53 hosted zone, yêu cầu chứng chỉ ACM và thiết lập Cognito để chuẩn bị endpoint truy cập an toàn cho hệ thống
- **Thiết lập nền tảng mạng**: Tạo VPC, target group, Application Load Balancer và security group để xây dựng đường đi mạng giữa người dùng và backend
- **Thiết lập pipeline và runtime container**: Chuẩn bị ECR, tích hợp pipeline build image với source code, sau đó triển khai ECS/Fargate để chạy backend
- **Thiết lập tầng dữ liệu**: Triển khai RDS để lưu trữ dữ liệu nghiệp vụ và Redis để tăng tốc truy xuất bằng cơ chế cache

Sau khi hoàn thành workshop, bạn sẽ có một môi trường LunchSync trên cloud với đầy đủ các thành phần cần thiết để phục vụ truy cập, xác thực, triển khai backend container, lưu trữ dữ liệu và tối ưu hiệu năng.
