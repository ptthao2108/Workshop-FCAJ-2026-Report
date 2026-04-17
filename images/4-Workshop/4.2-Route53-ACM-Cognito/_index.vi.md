---
title: "Route53, ACM và Cognito"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

#### Tổng quan

Mục này trình bày các bước cấu hình **Route 53**, **ACM** và **Cognito** cho lớp truy cập và xác thực của hệ thống LunchSync.

#### Các bước triển khai

1. Mở **Amazon Route 53**, vào mục **Hosted zones** và bắt đầu tạo hosted zone mới cho domain của workshop.

![Route53 bước 1](4.2.1-Route53-ACM/route53.1.jpg)

2. Tại màn hình **Create hosted zone**, nhập domain `lunchsync.space`, chọn loại **Public hosted zone** rồi tạo zone.

![Route53 bước 2](4.2.1-Route53-ACM/route53.2.jpg)

3. Sau khi zone được tạo, kiểm tra lại hai record mặc định **NS** và **SOA** của `lunchsync.space`.

![Route53 bước 3](4.2.1-Route53-ACM/route53.3.jpg)

4. Sao chép 4 nameserver trong record **NS** và cập nhật lại nameserver tại nhà đăng ký domain để Route 53 quản lý DNS cho `lunchsync.space`.

![Route53 bước 4](4.2.1-Route53-ACM/route53.4.jpg)

5. Chuyển sang **AWS Certificate Manager (ACM)** ở region sẽ dùng cho backend và bắt đầu yêu cầu chứng chỉ public.

![ACM bước 1](4.2.1-Route53-ACM/acm.1.jpg)

6. Chọn **Request a public certificate**, khai báo `lunchsync.space` và `*.lunchsync.space`, sau đó dùng **DNS validation**.

![ACM bước 2](4.2.1-Route53-ACM/acm.2.jpg)

7. Ở bước cấu hình chi tiết, giữ thuật toán khóa mặc định như `RSA 2048`, rà lại domain và gửi yêu cầu tạo certificate.

![ACM bước 3](4.2.1-Route53-ACM/acm.3.jpg)

8. Mở chi tiết certificate vừa tạo và kiểm tra các bản ghi **CNAME** dùng cho DNS validation trong Route 53.

![ACM bước 4](4.2.1-Route53-ACM/acm.4.jpg)

9. Nếu ACM hiển thị nút **Create records in Route 53**, tạo nhanh các record xác thực còn thiếu ngay từ giao diện ACM.

![ACM bước 5](4.2.1-Route53-ACM/acm.5.jpg)

10. Chờ certificate ở region ứng dụng chuyển sang trạng thái **Issued** rồi lưu lại ARN để dùng cho ALB.

![ACM bước 6](4.2.1-Route53-ACM/acm.6.jpg)

11. Lặp lại quy trình request certificate ở **US East (N. Virginia)** để có thêm một certificate dành riêng cho CloudFront, rồi xác nhận certificate này cũng ở trạng thái **Issued**.

![ACM bước 7](4.2.1-Route53-ACM/acm.7.jpg)

12. Mở **Amazon Cognito** và chọn **Create user pool** để tạo user pool cho LunchSync.

![Cognito bước 1](4.2.2-Cognito/co.1.jpg)

13. Chọn loại ứng dụng **Traditional web application**, đặt tên app là `Lunchsync-App`, dùng sign-in bằng `Email` và `Username`, bật **Self-registration** và giữ các thuộc tính đăng ký cần thiết như `email`, `name`.

![Cognito bước 2](4.2.2-Cognito/co.2.jpg)

14. Khai báo **Return URL** là `https://lunchsync.space/`, sau đó hoàn tất quick setup để Cognito tạo user pool và app client.

![Cognito bước 3](4.2.2-Cognito/co.3.jpg)

15. Sau khi tạo xong, mở lại user pool `Lunchsync - user pool` và xác nhận app `Lunchsync-App` đã được gắn vào pool.

![Cognito bước 4](4.2.2-Cognito/co.4.jpg)

16. Vào **App client** để lấy **Client ID**, kiểm tra **Client secret**, thời hạn token và các thuộc tính của ứng dụng.

![Cognito bước 5](4.2.2-Cognito/co.5.jpg)

17. Mở tab **Sign-in** của user pool để rà lại cách đăng nhập, MFA, device tracking và các thiết lập xác thực cơ bản.

![Cognito bước 6](4.2.2-Cognito/co.6.jpg)

18. Sửa lại **App client information** nếu cần, bảo đảm app client cho phép các flow mà ứng dụng cần như `ALLOW_USER_AUTH` và `ALLOW_REFRESH_TOKEN_AUTH`.

![Cognito bước 7](4.2.2-Cognito/co.7.jpg)

19. Chuyển sang **AWS Secrets Manager**, chọn **Other type of secret** và lưu các cặp key/value cho Cognito như `cognito_client_id`, `cognito_user_pool_id`, `cognito_client_secret`.

![Cognito bước 8](4.2.2-Cognito/co.8.jpg)

20. Đặt tên secret là `lunchsync/cognito` để backend có thể tham chiếu đúng secret này ở runtime.

![Cognito bước 9](4.2.2-Cognito/co.9.jpg)

21. Hoàn tất tạo secret và kiểm tra danh sách secrets để xác nhận `lunchsync/cognito` đã xuất hiện cùng các secret khác của hệ thống.

![Cognito bước 10](4.2.2-Cognito/co.10.jpg)
