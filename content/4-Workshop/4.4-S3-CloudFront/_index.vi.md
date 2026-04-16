---
title: "S3 và CloudFront"
date: 2026-04-04
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

#### Tổng quan

Mục này trình bày các bước cấu hình **S3**, **Origin Access Control** và **CloudFront** cho frontend tĩnh của hệ thống.

#### Các bước triển khai

1. Mở **Amazon S3** và bắt đầu tạo bucket mới cho frontend của LunchSync.

![S3 bước 1](/images/4-Workshop/4.4-S3-CloudFront/s3.1.jpg)

2. Đặt bucket prefix là `lunchsync-project`, chọn region `ap-southeast-1`, để S3 sinh tên đầy đủ theo account/region và giữ bucket ở trạng thái private.

![S3 bước 2](/images/4-Workshop/4.4-S3-CloudFront/s3.2.jpg)

3. Giữ `ACLs disabled`, bật **Block all public access**, không bật versioning và tiếp tục với bucket private.

![S3 bước 3](/images/4-Workshop/4.4-S3-CloudFront/s3.3.jpg)

4. Giữ mã hóa mặc định `SSE-S3`, tạo bucket, sau đó upload nội dung frontend vào các thư mục như `frontend/` và `media/`.

![S3 bước 4](/images/4-Workshop/4.4-S3-CloudFront/s3.4.jpg)

5. Sau khi bucket được gắn với CloudFront, kiểm tra **bucket policy** để chắc rằng chỉ service `cloudfront.amazonaws.com` của distribution được phép đọc object.

![S3 bước 5](/images/4-Workshop/4.4-S3-CloudFront/s3.5.jpg)

6. Chuyển sang **CloudFront > Origin access** và bắt đầu tạo một **Origin Access Control** mới cho bucket S3.

![OAC bước 1](/images/4-Workshop/4.4-S3-CloudFront/oac.1.jpg)

7. Đặt tên OAC là `lunchsync-oac`, mô tả rõ dùng cho bucket của LunchSync và giữ tùy chọn **Sign requests**.

![OAC bước 2](/images/4-Workshop/4.4-S3-CloudFront/oac.2.jpg)

8. Kiểm tra danh sách OAC để xác nhận `lunchsync-oac` đã được tạo xong trước khi sang bước dựng distribution.

![OAC bước 3](/images/4-Workshop/4.4-S3-CloudFront/oac.3.jpg)

9. Mở **CloudFront > Distributions** và bắt đầu tạo distribution mới.

![CloudFront bước 1](/images/4-Workshop/4.4-S3-CloudFront/cf.1.jpg)

10. Chọn plan khởi tạo distribution rồi tiếp tục wizard.

![CloudFront bước 2](/images/4-Workshop/4.4-S3-CloudFront/cf.2.jpg)

11. Đặt tên distribution là `S3-Frontend`, chọn kiểu **Single website or app** và bỏ qua bước gắn Route 53 domain ở wizard ban đầu.

![CloudFront bước 3](/images/4-Workshop/4.4-S3-CloudFront/cf.3.jpg)

12. Ở phần **Specify origin**, chọn loại origin là **Amazon S3**.

![CloudFront bước 4](/images/4-Workshop/4.4-S3-CloudFront/cf.4.jpg)

13. Chọn bucket S3 của project làm origin, đặt **Origin path** là `/frontend`, bật tùy chọn cho CloudFront truy cập bucket private.

![CloudFront bước 5](/images/4-Workshop/4.4-S3-CloudFront/cf.5.jpg)

14. Giữ các thiết lập origin/cache được AWS đề xuất cho nội dung tĩnh từ S3.

![CloudFront bước 6](/images/4-Workshop/4.4-S3-CloudFront/cf.6.jpg)

15. Ở bước **Enable security**, giữ các lớp bảo vệ mặc định của CloudFront/WAF trong wizard.

![CloudFront bước 7](/images/4-Workshop/4.4-S3-CloudFront/cf.7.jpg)

16. Rà lại trang review: origin S3 đang trỏ vào bucket LunchSync, `Origin path = /frontend`, rồi tạo distribution.

![CloudFront bước 8](/images/4-Workshop/4.4-S3-CloudFront/cf.8.jpg)

17. Sau khi distribution được tạo, mở tab **Origins** để xác nhận origin S3 đầu tiên đã được tạo với đúng origin path và đúng OAC.

![CloudFront bước 9](/images/4-Workshop/4.4-S3-CloudFront/cf.9.jpg)

18. Nếu cần, vào **Edit origin** để gắn lại `lunchsync-oac` và copy policy mẫu sang bucket policy của S3.

![CloudFront bước 10](/images/4-Workshop/4.4-S3-CloudFront/cf.10.jpg)

19. Mở tab **Behaviors** của distribution và chỉnh behavior mặc định cho frontend.

![CloudFront bước 11](/images/4-Workshop/4.4-S3-CloudFront/cf.11.jpg)

20. Với behavior mặc định, đặt **Viewer protocol policy = Redirect HTTP to HTTPS**.

![CloudFront bước 12](/images/4-Workshop/4.4-S3-CloudFront/cf.12.jpg)

21. Giữ cache policy phù hợp cho S3 như `CachingOptimized` và origin request policy kiểu `CORS-S3Origin`.

![CloudFront bước 13](/images/4-Workshop/4.4-S3-CloudFront/cf.13.jpg)

22. Mở tab **Error pages** và bắt đầu tạo custom error response cho SPA.

![CloudFront bước 14](/images/4-Workshop/4.4-S3-CloudFront/cf.14.jpg)

23. Tạo rule để khi origin trả về `403`, CloudFront phản hồi `/index.html` với mã `200` cho các route frontend.

![CloudFront bước 15](/images/4-Workshop/4.4-S3-CloudFront/cf.15.jpg)

24. Kiểm tra lại danh sách error pages để xác nhận rule SPA fallback đã được lưu.

![CloudFront bước 16](/images/4-Workshop/4.4-S3-CloudFront/cf.16.jpg)

25. Quay lại tab **Origins** và tạo thêm origin thứ hai là **ALB** của backend.

![CloudFront bước 17](/images/4-Workshop/4.4-S3-CloudFront/cf.17.jpg)

26. Với origin ALB, nhập DNS name của ALB, dùng giao thức **HTTPS only** để CloudFront nói chuyện với backend.

![CloudFront bước 18](/images/4-Workshop/4.4-S3-CloudFront/cf.18.jpg)

27. Tạo thêm origin S3 thứ ba cho thư mục media, vẫn dùng bucket cũ nhưng đặt **Origin path = /media** và gắn `lunchsync-oac`.

![CloudFront bước 19](/images/4-Workshop/4.4-S3-CloudFront/cf.19.jpg)

28. Kiểm tra lại tab **Origins** để xác nhận distribution hiện có 3 origin: `frontend`, `ALB backend` và `media`.

![CloudFront bước 20](/images/4-Workshop/4.4-S3-CloudFront/cf.20.jpg)

29. Tạo behavior mới cho đường dẫn `media/*`, trỏ về origin media trên S3 và vẫn redirect người dùng sang HTTPS.

![CloudFront bước 21](/images/4-Workshop/4.4-S3-CloudFront/cf.21.jpg)

30. Với behavior `media/*`:

![CloudFront bước 22](/images/4-Workshop/4.4-S3-CloudFront/cf.22.jpg)
![CloudFront bước 23](/images/4-Workshop/4.4-S3-CloudFront/cf.23.jpg)

31. Tạo behavior mới cho đường dẫn `api/*`, trỏ về origin ALB và HTTPS only, CachingDisable và origin request policy AllViewer.
    
32. Kiểm tra danh sách behavior cuối cùng 
![CloudFront bước 23](/images/4-Workshop/4.4-S3-CloudFront/cf.24.jpg)


33.  Quay về **Route 53**, mở hosted zone `lunchsync.space` và chuẩn bị tạo record alias cho CloudFront.
![CloudFront bước 26](/images/4-Workshop/4.4-S3-CloudFront/cf.26.jpg)

34. Tạo record alias ở root domain `lunchsync.space` trỏ đến distribution CloudFront.
![CloudFront bước 27](/images/4-Workshop/4.4-S3-CloudFront/cf.27.jpg)

35. Kiểm tra lại hosted zone để xác nhận record alias đến CloudFront đã xuất hiện.
![CloudFront bước 28](/images/4-Workshop/4.4-S3-CloudFront/cf.28.jpg)


36. Quay lại **CloudFront > Edit settings**, thêm **Alternate domain name (CNAME)** là `lunchsync.space` và chọn certificate ACM ở `us-east-1`.

![CloudFront bước 29](/images/4-Workshop/4.4-S3-CloudFront/cf.29.jpg)
![CloudFront bước 30](/images/4-Workshop/4.4-S3-CloudFront/cf.30.jpg)

1.  Trong cùng màn hình settings, đặt **Default root object = index.html** và bật các phiên bản HTTP cần thiết như `HTTP/2` và `HTTP/3`.

![CloudFront bước 31](/images/4-Workshop/4.4-S3-CloudFront/cf.31.jpg)

38. Lưu settings rồi kiểm tra trang tổng quan distribution để xác nhận alias `lunchsync.space`, certificate và `index.html` đã được áp dụng.

![CloudFront bước 32](/images/4-Workshop/4.4-S3-CloudFront/cf.32.jpg)

39. Mở `https://lunchsync.space` để kiểm tra frontend đã phục vụ qua CloudFront với domain riêng.

![CloudFront bước 33](/images/4-Workshop/4.4-S3-CloudFront/cf.33.jpg)

40. Kiểm tra thêm một asset như `https://lunchsync.space/media/lunchsync.png` để xác nhận behavior `media/*` cũng đang hoạt động đúng.

![CloudFront bước 34](/images/4-Workshop/4.4-S3-CloudFront/cf.34.jpg)
