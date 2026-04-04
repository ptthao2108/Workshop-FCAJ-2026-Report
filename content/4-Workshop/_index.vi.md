---
title: "Workshop"
date: 2026-03-20
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# LUNCHSYNC PLATFORM - AWS Infrastructure Workshop
Tài liệu này mô tả quy trình từng bước để triển khai hệ thống **LunchSync** lên AWS theo kiến trúc thực tế của dự án. Mục tiêu là thiết lập đầy đủ các thành phần hạ tầng cốt lõi gồm DNS, chứng chỉ SSL/TLS, mạng VPC, cân bằng tải, xác thực người dùng, phân phối nội dung tĩnh, cơ sở dữ liệu quan hệ và bộ nhớ đệm.

Chuỗi workshop được tổ chức theo đúng thứ tự triển khai: chuẩn bị domain và certificate trước, dựng networking và bảo mật, thiết lập authentication, sau đó hoàn thiện lớp dữ liệu và tối ưu hiệu năng để hệ thống sẵn sàng vận hành trên môi trường cloud.

#### Nội dung

1. [Tổng quan về workshop](4.1-Workshop-overview/)
2. [Route53 và ACM, Cognito](4.2-Route53-ACM-Cognito/)
3. [VPC, Security Group và ALB](4.3-VPC-SecurityGroup-ALB/)
4. [S3 và CloudFront](4.4-S3-CloudFront/)
5. [Lưu trữ dữ liệu](4.5-Database-Caching/)
6. [Vận hành Backend](4.6-ECR-Fargate/)
7. [Dọn dẹp tài nguyên](4.7-Cleanup/)