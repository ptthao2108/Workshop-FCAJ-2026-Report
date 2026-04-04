---
title: "Chuẩn bị private path, ECR và GitHub Actions"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.6.1. </b> "
---

#### Tổng quan

Mục này trình bày các bước chuẩn bị private path, **ECR repository** và **GitHub Actions** cho quy trình build và push image.

#### Các bước triển khai

1. Trước tiên, tạo security group `vpc-endpoints-sg` trong `lunchsync-vpc`, cho phép inbound từ `backend-sg` và outbound toàn phần để dùng cho các interface endpoint.

![VPC bước 0](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.0.jpg)

2. Mở **VPC > Endpoints** và bắt đầu tạo interface endpoint đầu tiên.

![VPC bước 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.1.jpg)

3. Chọn service như `com.amazonaws.ap-southeast-1.ecr.api`, bật **Private DNS**, gắn vào `lunchsync-vpc` và chọn 2 private subnet.

![VPC bước 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.2.jpg)

4. Gắn security group `vpc-endpoints-sg`, giữ endpoint policy là `Full access`, rồi tạo endpoint.

![VPC bước 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.3.jpg)

5. Lặp lại mẫu cấu hình này cho các endpoint backend cần dùng như `ecr.dkr`, `secretsmanager`, `logs`, `cognito-idp` và kiểm tra danh sách endpoint sau khi tạo.

![VPC bước 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.4.jpg)

6. Chuyển sang **Amazon ECR** và bắt đầu tạo private repository cho backend.

![ECR bước 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.1.jpg)

7. Đặt tên repository là `lunchsync-backend`.

![ECR bước 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.2.jpg)

8. Cấu hình repository với tag immutability, encryption mặc định của ECR/KMS và giữ các lựa chọn scan phù hợp cho luồng CI/CD.

![ECR bước 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.3.jpg)

9. Sau khi tạo xong, mở repository `lunchsync-backend` để kiểm tra các tab cấu hình chính.

![ECR bước 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.4.jpg)

10. Tạo lifecycle policy đầu tiên để dọn các image **untagged** cũ.

![ECR bước 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.5.jpg)

11. Tạo thêm rule để chỉ giữ lại số lượng image **tagged** gần nhất cần thiết cho việc rollback.

![ECR bước 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.6.jpg)

12. Rà lại lifecycle policy sau khi thêm đủ rule và xác nhận policy đã được lưu vào repository.

![ECR bước 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.7.jpg)

13. Kiểm tra tab lifecycle policy một lần nữa để chắc rằng repository đang tự dọn ảnh cũ đúng hướng mong muốn.

![ECR bước 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.8.jpg)

14. Từ danh sách repository, mở **View push commands** để lấy URI chuẩn cho lệnh login, build, tag và push.

![ECR bước 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.9.jpg)

15. Lưu lại toàn bộ push commands của ECR vì chính các lệnh này sẽ được đưa vào workflow hoặc dùng khi test thủ công.

![ECR bước 10](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.10.jpg)

16. Khi phần ECR đã sẵn sàng, chuyển sang **IAM > Identity providers** để thêm OIDC provider cho GitHub Actions.

![Git bước 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.1.jpg)

17. Chọn loại provider là **OpenID Connect**, nhập URL `https://token.actions.githubusercontent.com` và audience `sts.amazonaws.com`.

![Git bước 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.2.jpg)

18. Kiểm tra lại danh sách provider để xác nhận provider GitHub đã xuất hiện.

![Git bước 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.3.jpg)

19. Tạo role mới với trusted entity là **Web identity** để GitHub Actions có thể assume role.

![Git bước 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.4.jpg)

20. Ràng buộc trust relationship theo đúng GitHub organization `pttha02108`, repository `lunchsync` và branch `main`.

![Git bước 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.5.jpg)

21. Đặt tên role là `lunchsync-cicd-role` rồi tạo role.

![Git bước 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.6.jpg)

22. Mở role vừa tạo và bắt đầu tạo inline policy cho pipeline.

![Git bước 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.7.jpg)

23. Trong policy JSON, cấp quyền cho pipeline đăng nhập ECR, push image, đăng ký task definition mới, cập nhật ECS service và thao tác với các tài nguyên liên quan.

![Git bước 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.8.jpg)

24. Đặt tên policy, ví dụ `lunchsync-cicd-policy`, rồi lưu lại.

![Git bước 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.9.jpg)

25. Kiểm tra lại role `lunchsync-cicd-role` để chắc rằng policy đã được attach đầy đủ.

![Git bước 10](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.10.jpg)

26. Quay về GitHub repository, vào **Settings > Secrets and variables > Actions**.

![Git bước 11](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.11.jpg)

27. Tạo các repository secrets như `AWS_REGION`, `AWS_ROLE_ARN`, `ECR_REPOSITORY`, `ECS_CLUSTER`, `ECS_SERVICE`.

![Git bước 12](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.12.jpg)

28. Kiểm tra thư mục `.github/workflows` để xác nhận đã có các workflow như `ci.yml` và `cd.yml`.

![Git bước 13](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.13.jpg)

29. Thử lệnh `docker login` vào ECR và `docker build` image backend từ `LunchSync.Api/Dockerfile`.

![ECR bước 11](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.11.jpg)

30. Tag image theo đúng URI của repository `lunchsync-backend` rồi `docker push` lên ECR.

![ECR bước 12](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.12.jpg)

31. Quay lại ECR để xác nhận image/tag mới đã xuất hiện trong repository.

![ECR bước 13](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.13.jpg)

32. Mở chi tiết image để lấy digest và URI đầy đủ phục vụ task definition.

![ECR bước 14](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.14.jpg)

33. Kiểm tra lại toàn bộ danh sách image, tag và thời gian push để chắc rằng repository đã sẵn sàng cho bước ECS.

![ECR bước 15](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.15.jpg)
