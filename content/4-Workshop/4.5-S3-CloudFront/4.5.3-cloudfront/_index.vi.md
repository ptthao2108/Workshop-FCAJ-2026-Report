---
title: "Tạo CloudFront distribution"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 4.5.3. </b> "
---

1. Mở **CloudFront console** và bắt đầu tạo distribution mới.

![CloudFront bước 1](cf.1.jpg)

![CloudFront bước 2](cf.2.jpg)

![CloudFront bước 3](cf.3.jpg)

![CloudFront bước 4](cf.4.jpg)

2. Chọn S3 bucket làm origin và kiểm tra các thông tin origin.

![CloudFront bước 5](cf.5.jpg)

![CloudFront bước 6](cf.6.jpg)

![CloudFront bước 7](cf.7.jpg)

![CloudFront bước 8](cf.8.jpg)

3. Cấu hình viewer settings và cache behavior cho distribution.

![CloudFront bước 9](cf.9.jpg)

![CloudFront bước 10](cf.10.jpg)

![CloudFront bước 11](cf.11.jpg)

![CloudFront bước 12](cf.12.jpg)

4. Gắn OAC đã tạo trước đó và cập nhật policy truy cập của S3 nếu cần.

![CloudFront bước 13](cf.13.jpg)

![CloudFront bước 14](cf.14.jpg)

![CloudFront bước 15](cf.15.jpg)

![CloudFront bước 16](cf.16.jpg)

5. Tiếp tục cấu hình các tuỳ chọn như default root object và protocol hỗ trợ.

![CloudFront bước 17](cf.17.jpg)

![CloudFront bước 18](cf.18.jpg)

![CloudFront bước 19](cf.19.jpg)

![CloudFront bước 20](cf.20.jpg)

6. Rà soát các thiết lập nâng cao trước khi gửi tạo distribution.

![CloudFront bước 21](cf.21.jpg)

![CloudFront bước 22](cf.22.jpg)

![CloudFront bước 23](cf.23.jpg)

![CloudFront bước 24](cf.24.jpg)

7. Tạo distribution và chờ trạng thái deploy được cập nhật.

![CloudFront bước 25](cf.25.jpg)

![CloudFront bước 26](cf.26.jpg)

![CloudFront bước 27](cf.27.jpg)

![CloudFront bước 28](cf.28.jpg)

8. Kiểm tra domain name của distribution, thử truy cập nội dung và xem lại kết quả cuối cùng.

![CloudFront bước 29](cf.29.jpg)

![CloudFront bước 30](cf.30.jpg)

![CloudFront bước 31](cf.31.jpg)

![CloudFront bước 32](cf.32.jpg)

![CloudFront bước 33](cf.33.jpg)

![CloudFront bước 34](cf.34.jpg)
