---
title: "Triển khai IAM, ECS và task definition"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.6.2. </b> "
---

#### Tổng quan

Mục này trình bày các bước cấu hình **IAM**, **ECS** và **Task Definition** cho backend trên **Fargate**.

#### Các bước triển khai

1. Mở **IAM > Roles** để chuẩn bị các role cho ECS.

![IAM bước 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.1.jpg)

2. Tạo role mới với trusted entity là **AWS service**, service là **Elastic Container Service** và use case **Elastic Container Service Task**.

![IAM bước 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.2.jpg)

3. Đặt tên role là `lunchsync-ecs-task-role` để dùng làm **task role** cho container backend và không add permissions.

![IAM bước 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.3.jpg)
![IAM bước 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.4.jpg)

4. Tạo role thứ hai cho **execution role**, ở bước Add permissions chọn policy managed `AmazonECSTaskExecutionRolePolicy`.

![IAM bước 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.7.jpg)

5. Thêm inline policy cho execution role để ECS agent cũng có thể đọc các secret cần inject vào container khi task khởi động.

![IAM bước 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.8.jpg)

6. Kiểm tra lại role `lunchsync-ecs-execution-role` để chắc rằng nó có cả `AmazonECSTaskExecutionRolePolicy` và policy đọc secret.

![IAM bước 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.9.jpg)

7. Mở **Amazon ECS** và vào mục **Clusters**.

![ECS bước 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.1.jpg)

8. Tạo cluster mới tên `lunchsync-cluster`, chọn hạ tầng **Fargate only**.

![ECS bước 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.2.jpg)

9. Nếu cần, cấu hình thêm phần observability, **Container Insights** và **ECS Exec** theo mức theo dõi mong muốn.

![ECS bước 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.3.jpg)

10. Hoàn tất tạo cluster.

![ECS bước 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.4.jpg)

11. Mở cluster `lunchsync-cluster` và xác nhận cluster đang ở trạng thái **Active** với capacity provider Fargate.

![ECS bước 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.5.jpg)

12. Từ ECS, chuyển sang **Task definitions** và bắt đầu tạo task definition mới cho backend.

![ECS bước 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.6.jpg)

13. Đặt family là `lunchsync-backend`, chọn **AWS Fargate**, `Linux/X86_64`, `awsvpc`, gán `lunchsync-ecs-task-role` và `lunchsync-ecs-execution-role`.

![Task Definition bước 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.1.jpg)

14. Thêm container `lunchsync-backend`, dùng image URI từ ECR và mở port `8080`.

![Task Definition bước 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.2.jpg)

15. Khai báo environment variables và secret mapping cho Cognito, chuỗi kết nối database, Redis, `NODE_ENV` và các biến runtime cần thiết khác.

![Task Definition bước 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.3.jpg)

16. Cấu hình health check cho container, ví dụ `curl -f http://localhost:8080/health || exit 1`.

![Task Definition bước 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.4.jpg)

17. Tạo revision task definition và xác nhận `lunchsync-backend` đã ở trạng thái **ACTIVE**.

![Task Definition bước 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.5.jpg)

18. Quay lại cluster `lunchsync-cluster` và bắt đầu tạo **service** mới dùng task definition vừa tạo.

![ECS bước 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.7.jpg)

19. Đặt service name là `lunchsync-backend-service`, chọn launch type **FARGATE** và task definition family `lunchsync-backend`.

![ECS bước 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.8.jpg)

20. Cấu hình networking cho service: chọn private subnets trong `lunchsync-vpc`, gắn `backend-sg` và để `Public IP = Off`.

![ECS bước 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.9.jpg)

21. Bật load balancing bằng **Application Load Balancer**, dùng ALB hiện có, listener `HTTPS:443` và container `lunchsync-backend:8080`.

![ECS bước 10](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.10.jpg)

22. Gắn service với target group `lunchsync-tg-backend`, giữ health check path `/health`, rồi tạo service.

![ECS bước 11](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.11.jpg)

23. Sau khi tạo xong, theo dõi rollout ban đầu: service có `2 Desired tasks`, deployment còn **In progress** và target health cần vài phút để ổn định.

![ECS bước 12](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.12.jpg)

24. Nếu muốn tự động scale, cấu hình **Service Auto Scaling** theo CPU utilization.

![ECS bước 13](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.13.jpg)

25. Có thể thêm policy auto scaling thứ hai theo memory utilization.

![ECS bước 14](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.14.jpg)

26. Ở màn hình cuối, kiểm tra tab **Tasks** để xác nhận task đã chạy; nếu service vẫn báo trạng thái như `Rollback failed` hoặc target health chưa xanh thì cần mở **Events/Logs** để rà lại cấu hình container, secret hoặc health check.

![ECS bước 15](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.15.jpg)
