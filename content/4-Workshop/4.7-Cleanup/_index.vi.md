---
title: "Dọn dẹp tài nguyên"
date: 2026-04-04
weight: 7
chapter: false
pre: " <b> 4.7. </b> "
---

#### Tổng quan

Mục này trình bày trình tự dọn dẹp các tài nguyên đã tạo cho môi trường demo của hệ thống **LunchSync**.

#### Giai đoạn 1: Dọn dẹp Backend Runtime và CI/CD

Giai đoạn này tập trung vào các tài nguyên dùng để chạy backend, lưu trữ image và kết nối pipeline triển khai.

##### 1.1 Xóa ECS Service

Vào **ECS Console** → **Clusters**.  
Mở cluster backend của LunchSync.  
Trong tab **Services**, chọn service backend và nhấn **Delete service**.

##### 1.2 Gỡ các Task Definition Revision

Vào **ECS Console** → **Task definitions**.  
Chọn family: `lunchsync-backend`.  
Chọn các revision đã tạo và thực hiện **Deregister**.

##### 1.3 Xóa ECR Repository

Vào **Amazon ECR** → **Repositories**.  
Chọn repository: `lunchsync-backend`.  
Xóa các image còn lại nếu cần, sau đó nhấn **Delete repository**.

##### 1.4 Xóa IAM Role cho GitHub Actions

Vào **IAM Console** → **Roles**.  
Chọn role: `lunchsync-cicd-role`.  
Nhấn **Delete** để xóa role dùng cho pipeline CI/CD.

##### 1.5 Xóa OIDC Identity Provider

Vào **IAM Console** → **Identity providers**.  
Chọn provider: `token.actions.githubusercontent.com`.  
Nhấn **Delete** nếu provider này chỉ được tạo cho workshop.

##### 1.6 Xóa Secrets hoặc Variables trên GitHub

Vào **GitHub Repository** → **Settings** → **Secrets and variables** → **Actions**.  
Xóa các biến hoặc secret đã tạo cho workflow như `AWS_REGION`, `AWS_ROLE_ARN`, `ECR_REPOSITORY`, `ECS_CLUSTER`, `ECS_SERVICE`.

#### Giai đoạn 2: Dọn dẹp Frontend và Lớp Truy cập

Giai đoạn này bao gồm các tài nguyên phục vụ truy cập công khai như CloudFront, S3, Route 53, ACM và Cognito.

##### 2.1 Xóa CloudFront Distribution

Vào **CloudFront Console** → **Distributions**.  
Chọn distribution của frontend LunchSync.  
Thực hiện **Disable** nếu cần, sau đó nhấn **Delete**.

##### 2.2 Làm trống và xóa S3 Bucket

Vào **S3 Console**.  
Chọn bucket chứa frontend build của LunchSync.  
Trong tab **Objects**, nhấn **Empty** để xóa toàn bộ dữ liệu, sau đó quay lại danh sách bucket và nhấn **Delete**.

##### 2.3 Xóa Route 53 Record và Hosted Zone

Vào **Route 53 Console** → **Hosted zones**.  
Mở hosted zone đã dùng cho môi trường demo.  
Xóa các record trỏ tới **CloudFront** hoặc **ALB**, sau đó xóa hosted zone nếu không tiếp tục sử dụng.

##### 2.4 Xóa ACM Certificate

Vào **AWS Certificate Manager**.  
Chọn certificate đã cấp cho domain frontend hoặc backend của LunchSync.  
Nhấn **Delete** để xóa chứng chỉ.

##### 2.5 Xóa Cognito User Pool

Vào **Amazon Cognito** → **User pools**.  
Chọn user pool đã tạo cho LunchSync.  
Nhấn **Delete user pool** để xóa toàn bộ cấu hình xác thực của môi trường demo.

#### Giai đoạn 3: Dọn dẹp Tầng Dữ liệu và Cấu hình Bảo mật

Giai đoạn này áp dụng cho cơ sở dữ liệu, bộ nhớ đệm và các secret dùng trong runtime.

##### 3.1 Xóa Redis Cluster

Vào **ElastiCache Console** → **Redis OSS** hoặc **Valkey/Redis clusters**.  
Chọn cluster Redis đã tạo cho LunchSync.  
Nhấn **Delete** để xóa cụm cache.

##### 3.2 Xóa RDS Database Instance

Vào **RDS Console** → **Databases**.  
Chọn database instance của LunchSync.  
Nhấn **Delete** và chọn phương án lưu hoặc bỏ final snapshot theo yêu cầu lưu trữ của môi trường demo.

##### 3.3 Xóa Secrets Manager Secrets

Vào **AWS Secrets Manager**.  
Chọn các secret đã tạo cho backend như thông tin **Cognito**, **database**, **Redis** hoặc các biến runtime khác.  
Thực hiện **Delete** đối với từng secret không còn sử dụng.

#### Giai đoạn 4: Dọn dẹp Network Foundation

Giai đoạn cuối cùng xử lý các tài nguyên mạng đã tạo cho ALB, backend và lớp dữ liệu.

##### 4.1 Xóa Application Load Balancer

Vào **EC2 Console** → **Load Balancers**.  
Chọn **Application Load Balancer** của LunchSync.  
Nhấn **Delete load balancer**.

##### 4.2 Xóa Target Group

Vào **EC2 Console** → **Target Groups**.  
Chọn target group backend của LunchSync.  
Nhấn **Delete**.

##### 4.3 Xóa VPC Endpoints và NAT Gateway

Vào **VPC Console** → **Endpoints**.  
Xóa các endpoint đã tạo cho private path của backend.  
Nếu workshop có tạo **NAT Gateway**, vào **NAT Gateways** và xóa tài nguyên tương ứng.

##### 4.4 Xóa Security Groups

Vào **EC2 Console** → **Security Groups**.  
Xóa các security group đã tạo riêng cho **ALB**, **backend**, **RDS**, **Redis** và các endpoint sau khi các tài nguyên phụ thuộc đã được gỡ.

##### 4.5 Xóa VPC

Vào **VPC Console** → **Your VPCs**.  
Chọn VPC đã tạo cho môi trường LunchSync.  
Nhấn **Delete VPC** để hoàn tất quá trình dọn dẹp.
