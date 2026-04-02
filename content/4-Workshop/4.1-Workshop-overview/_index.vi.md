---
title : "Giới thiệu"
date : 2026-03-20 
weight : 1
chapter : false
pre : " <b> 4.1. </b> "
---

#### Tổng quan Workshop
Workshop này hướng dẫn xây dựng và triển khai một hệ thống ứng dụng web hiện đại (LunchSync) trên nền tảng AWS, tập trung vào kiến trúc container-based serverless và quy trình CI/CD tự động hóa.

- Lộ trình nội dung chính bao gồm:

    - Thiết lập hạ tầng mạng & Bảo mật: Sử dụng VPC với các Private Subnet, bảo vệ ứng dụng bằng AWS WAF và xác thực người dùng qua Amazon Cognito.

    - Triển khai Frontend: Hosting trang web tĩnh trên S3, phân phối qua CloudFront để tối ưu tốc độ và quản lý tên miền bằng Route 53.

    - Vận hành Backend: Sử dụng container hóa với Docker, lưu trữ trên ECR và chạy trên AWS Fargate (ECS) để tự động co giãn theo tải thực tế.

    - Lưu trữ dữ liệu: Kết nối với cơ sở dữ liệu PostgreSQL và bộ nhớ đệm Redis để tối ưu hiệu năng truy vấn.

    - Tự động hóa (DevOps): Xây dựng luồng CI/CD hoàn chỉnh qua GitHub Actions, giúp tự động build code, đẩy ảnh container và cập nhật dịch vụ ngay khi có thay đổi từ phía lập trình viên.

#### Giới thiệu hạ tầng
- **Amazon ALB (Application Load Balancer)**: Phân phối lưu lượng HTTP/HTTPS đến các service backend.  
- **Amazon ECS Fargate**: Chạy container backend theo mô hình serverless.  
- **Amazon S3**: Lưu trữ static assets và host frontend.  
- **Amazon CloudFront**: CDN phân phối nội dung frontend và hình ảnh với độ trễ thấp.  
- **Amazon Cognito**: Quản lý xác thực và phân quyền người dùng.  
- **WAF tích hợp CloudFront**: Bảo vệ hệ thống khỏi các tấn công web phổ biến.  
- **Amazon RDS (PostgreSQL)**: Lưu trữ dữ liệu quan hệ (user, restaurant, session, v.v.).  
- **Amazon ElastiCache (Redis)**: Cache dữ liệu nóng, xử lý voting và giảm tải hệ thống.  
- **Amazon CloudWatch**: Giám sát hệ thống (logs, metrics, alarms).  
- **Amazon ECR**: Lưu trữ Docker images phục vụ deployment.  
- **AWS IAM**: Quản lý quyền truy cập giữa các service và pipeline CI/CD. 


