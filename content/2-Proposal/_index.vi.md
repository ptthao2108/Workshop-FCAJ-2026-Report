---
title: "Bản đề xuất"
date: 2026-03-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Lunch Sync PLATFORM for Lunch Decision  
## Giải pháp Cloud-Native trên AWS hỗ trợ ra quyết định món ăn  

### 1. Tóm tắt điều hành  
- **LunchSync** là một **Nền tảng Hỗ trợ ra quyết định theo nhóm** theo thời gian thực, được thiết kế chuyên biệt cho nhân viên văn phòng tại khu vực đô thị mật độ cao (Quận 1, TP.HCM). Dự án giải quyết bài toán **"trưa nay ăn gì"** bằng cách số hóa quy trình thỏa hiệp của nhóm thông qua **cơ chế bỏ phiếu nhanh (Gamified Voting)** và thuật toán **gợi ý dựa trên ngữ cảnh (Context-aware)**.

- Khác với các ứng dụng Food Delivery tập trung vào vấn đề logistic, **LunchSync không bán đồ ăn, mà bán "quyết định"**,  giải quyết bài toán **Mâu thuẫn sở thích** cùng **Mệt mỏi khi ra quyết định** vào giờ nghỉ trưa. Với kiến trúc **container-based serverless (Fargate) trên AWS**, dự án đảm bảo khả năng **xử lý lượng truy cập cao vào các giờ cao điểm**, **giảm thời gian chọn** quán từ 20 phút xuống còn **3 phút**, **tối ưu hóa giờ nghỉ trưa** quý giá của người dùng.  

### 2. Tuyên bố vấn đề  
#### Vấn đề hiện tại  
- **Decision fatigue:** Sau giờ làm căng thẳng, mọi người **không còn năng lượng để nghĩ “ăn gì”**.  
- **Conflict of preferences:** Nhóm nhiều người → **mỗi người một khẩu vị → khó chốt**.  
- **Resource wastage:** Mất 15–20 phút chọn quán → **giảm thời gian nghỉ trưa, ảnh hưởng năng suất**.  
- **Social friction thấp:** Quyết định ăn chung **không bị áp lực trách nhiệm cá nhân**, vì kết quả là trải nghiệm chung của cả nhóm.  

#### Giải pháp  
- **Core idea:** Ứng dụng mobile tập trung vào **real-time voting + elimination game**, giúp nhóm **chốt quán ăn trong ~90 giây**.

- **Cách hoạt động:**
  - Tạo **session** nhanh.
  - Người dùng chọn tiêu chí qua **binary choices (10 Preference dimension of food)** - tiêu chí khẩu vị thay vì tranh luận món.
  - Hệ thống dùng:
    - **Weighted Scoring** → lọc & xếp hạng quán.
    - **"Boom" (random elimination)** → cưỡng chế ra quyết định cuối cùng.
> Người dùng chọn sở thích qua 10 chiều đặc tính món ăn (food preference dimensions): độ nước, nhiệt độ, độ nặng bụng, vị đậm nhạt, cay, kết cấu, thời gian ăn, độ mới lạ, healthy, ăn chung/riêng.
- **Mục tiêu MVP:**
  - Xử lý **concurrency cao** (nhiều user vote cùng lúc).
  - Xử lý **geospatial data chính xác** (gom quán theo landmark/area).
  - **UX tối giản**, tập trung tốc độ ra quyết định.

- **Technical implementation:**
  - Kiến trúc: **Modular Monolith**.
  - Design: **Clean Architecture** → dễ maintain & mở rộng.
  - Backend: **ASP.NET**.
  - Hạ tầng: triển khai hoàn toàn trên **AWS**.

> MVP real-time sử dụng **short polling (3s interval)** thay vì WebSocket 
> để giảm complexity. WebSocket sẽ được xem xét ở phase 2.
> 
#### Lợi ích và hoàn vốn đầu tư (ROI)  
- **Bảo mật & ổn định:** Private Subnet + Auto Scaling → đảm bảo **high availability**, giảm thiểu downtime.  
- **Hiệu năng:** Khả năng scale linh hoạt theo giờ cao điểm, độ trễ thấp nhờ CDN và caching.  
- **Tập trung phát triển:** Sử dụng managed services → giảm gánh nặng vận hành (Ops), tăng tốc phát triển MVP.  

- **Chi phí:**  
  - Nhóm cốt lõi: ~$78.42/tháng (~2.000.000 VNĐ) cho Fargate, RDS, ElastiCache với Multi-AZ.
  - Nhóm Hạ tầng & Bảo mật (~$131/tháng): 
    - VPC Endpoints ($94.90): "Đường ống" bảo mật nội bộ.
    - ALB & Public IPs (~$24.00): "Cửa ngõ" đón khách.
    - Khác (WAF, Logs, Route53...): ~$12.00.   
  
#### ROI Summary (SaaS)
+ > Bảng ROI dưới đây là **giả định scaling** khi chuyển sang 
+ > mô hình SaaS/Freemium sau khi đạt product-market fit.

| Chỉ số                    | Giá trị         |
| ------------------------- | --------------- |
| Vốn đầu tư ban đầu        | $5.000          |
| Chi phí AWS / tháng       | ~$210            |
| Doanh thu (100 users)     | $2.000 / tháng  |
| Lợi nhuận ròng            | ~$1.290 / tháng |
| Thời gian hoàn vốn        | ~3.9 tháng      |
| ROI (1 năm)               | ~309%           |
| Doanh thu (1.000 users)   | $20.000 / tháng |
| Chi phí AWS (1.000 users) | ~$350 / tháng   |
| Biên lợi nhuận            | ~93% - 98%     |
  > Với chi phí ~$210.12/tháng, hệ thống đạt mức **High Availability (Multi-AZ)** và khả năng **failover tự động**. Khi một Availability Zone gặp sự cố, hệ thống sẽ chuyển sang AZ còn lại trong vài giây, đảm bảo dịch vụ hoạt động liên tục.   

### 3. Kiến trúc giải pháp  
Hệ thống được xây dựng trên nền tảng Amazon Web Services theo kiến trúc cloud-native, sử dụng Amazon ECS Fargate để triển khai backend theo mô hình container-based serverless, không cần quản lý hạ tầng.

Ứng dụng được triển khai theo mô hình Multi-AZ kết hợp Auto Scaling nhằm đảm bảo High Availability và khả năng mở rộng linh hoạt trong giờ cao điểm. Lưu lượng truy cập được phân phối thông qua Application Load Balancer (ALB).

Frontend được lưu trữ trên Amazon S3 và phân phối qua CloudFront CDN để tối ưu độ trễ. Hệ thống bảo mật bằng AWS WAF, Amazon Cognito và Security Groups nhằm kiểm soát truy cập và bảo vệ ứng dụng.

Dữ liệu được lưu trữ trên Amazon RDS (PostgreSQL), trong khi Amazon ElastiCache (Redis) được sử dụng để cache và xử lý các tác vụ độ trễ thấp như session và voting, giúp giảm tải cho backend. Hệ thống được giám sát bằng Amazon CloudWatch và triển khai tự động thông qua CI/CD với GitHub Actions, kết hợp Amazon ECR và AWS IAM.   

![LunchSync Platform Architecture](/images/2-Proposal/lunchsync.png)

#### Dịch vụ AWS sử dụng  
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
- **AWS Secrets Manager**: Quản lý các tài khoản, mật khẩu kết nối đến Cognito, RDS, Redis 
- **VPC Endpoint**: Kết nối an toàn các tài nguyên trong ptrivate subnet vs dịch vụ ngoài vpc

### 4. Scope MVP

| Aspect             | In Scope                                                                | Out of Scope                     |
| ------------------ | ----------------------------------------------------------------------- | -------------------------------- |
| Meal type          | Lunch only                                                              | Dinner, breakfast, cafe          |
| Platform           | React Web App (mobile-first)                                            | Native mobile app                |
| Backend            | ASP.NET Pragmatic Modular Monolith                                      | Microservices                    |
| Database           | PostgreSQL                                                              | NoSQL                            |
| Vector computation | Trong .NET (math cơ bản)                                                | Python external service          |
| Group size         | 3–8 người                                                               | >8                               |
| Auth               | Host cần account (simple JWT - Cognito); Participant join bằng nickname | Social login (if time available) |
| Data               | Manual curation                                                         | Auto-crawling                    |
| Geography          | 1 khu vực (3–5 collections)                                             | Multi-city                       |
| Monetization       | Free toàn bộ                                                            | Paid plans                       |
| Dietary            | Không filter                                                            | Chay, dị ứng, halal              |
| Restaurant action  | Google Maps link                                                        | Đặt bàn, review, gọi quán        |
### 5. Tech Stack
| Layer           | Technology                               | Hosting                         |
| --------------- | ---------------------------------------- | ------------------------------- |
| Frontend        | React + TypeScript + Vite + Tailwind CSS | S3 + CloudFront                 |
| Backend         | ASP.NET Web API + EF Core                | ECS Fargate                     |
| Database        | PostgreSQL 15+                           | AWS RDS (db.t3.micro Free Tier) |
| Media Storage   | —                                        | AWS S3                          |
| Auth (Phase 1)  | Simple JWT                               | Amazon Cognito                  |
| CI/CD           | GitHub Actions                           | Build + Deploy                  |
| Version Control | Git — GitHub                             | —                               |

### 6. Key Features

Group Voting Lunch Restaurant Flow 
PHASE 0: SETUP (1 người host)
- Tạo group session (3–8 người)
- Set trần giá (ví dụ 50k/người)
- Chọn Landmark-based Lunch Collection → load top 3 collection và chọn

PHASE 1: INDIVIDUAL BINARY CHOICE
- Cung cấp một bộ lấy từ trong pool binary choice có sẵn(20-30)
- Mỗi người tham gia trả lời cùng một bộ fixed 7–12 binary choice
- Hoàn thành trả lời bộ binary choice, từ mỗi participant tạo ra 1 preference vector có các tiêu chí bị ảnh hưởng bởi từng lựa chọn trong khi trả lời.

PHASE 2: AGGREGATE & SUGGEST (hệ thống xử lý)
- Gộp các preference vectors thành final vector đại diện cho session đó
- Scoring toàn bộ dish pool có top 10 dishes
- **Filter:** Chỉ giữ dishes mà tồn tại quán trong collection có phục vụ
- Đưa ra top 3 món

PHASE 3: RESTAURANT MATCHING (hệ thống gợi ý)
- Matching và ranking quán theo: Collection + trần giá
- Gợi ý top 5 quán phù hợp có cover (nhiều) món trong top.

PHASE 4: GROUP PICK
- Từ top 5 quán → nhóm veto 2 quán → quyết định chốt 1 quán cuối cùng để đi ăn.
- Có thể voting hoặc host quyết 

### 7. Lộ trình & Mốc triển khai  
- *Thực tập (Tháng 1–3)*:  
    - Tháng 1: Học AWS tìm hiểu hạ tầng và kỹ thuật.  
    - Tháng 2: Thiết kế và điều chỉnh kiến trúc cho phù hợp.  
    - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.  
- *Sau triển khai*: Nghiên cứu thêm trong vòng 1 năm.  

### 8. Ước tính ngân sách  

### Bảng Chi Phí Hệ Thống Full 2 AZ (30 ngày)

| Nhóm Dịch Vụ  | Thành phần (Multi-AZ)      | Cách tính chi tiết                          | Chi phí (USD/Tháng)    |
| ------------- | -------------------------- | ------------------------------------------- | ---------------------- |
| VPC Endpoint  | 5 Interface Endpoints      | 5 loại × 2 AZs × 730h × $0.013              | $94.90                 |
| Load Balancer | ALB + 2 Public IPs         | ($0.0225 + $0.01 cho 2 IP) × 730h           | $23.72                 |
| Compute       | ECS Fargate                | 2 Tasks (0.25 vCPU - 0.5GB)                 | $19.00                 |
| Database      | RDS PostgreSQL             | t3.micro (Multi-AZ: 2 nodes + 20GB Storage) | $30.00 - $34.00        |
| Cache         | ElastiCache Redis          | t3.micro (Multi-AZ: 2 nodes)                | $24.00 - $26.00        |
| Monitoring    | CloudWatch + WAF + Secrets | Lưu lượng thấp/cơ bản                       | $10.50                 |
| Khác          | Route53, S3, ECR, Cognito  | Phí duy trì                                 | $2.00                  |
| **TỔNG CỘNG** |                            |                                             | **~$204.12 - $210.12** |

---

### Đánh giá

- **Mức độ chịu lỗi:** Cực cao. Hệ thống có thể chịu được việc một Availability Zone (AZ) của AWS bị gián đoạn hoàn toàn mà ứng dụng vẫn hoạt động bình thường.

- **Bảo mật:** Tối ưu cao trên Cloud hiện nay (sử dụng private subnet hoàn toàn, không có Public IP, toàn bộ traffic đi qua PrivateLink).

- **Chi phí:** Khoảng **5,2 triệu VNĐ/tháng**.    

### 9. Đánh giá rủi ro  
| Rủi ro                                  | Mức độ     | Chiến lược giảm thiểu (Mitigation) | Chiến lược dự phòng (Contingency)    |
| --------------------------------------- | ---------- | ---------------------------------- | ------------------------------------ |
| **Hạ tầng / Uptime** (AZ failure)       | Thấp       | Multi-AZ + ALB failover            | Không cần (AWS tự xử lý)             |
| **Bảo mật** (DDoS, SQLi)                | Thấp       | WAF + Cognito + Private Subnet     | Rotate credentials, isolate system   |
| **Lỗi triển khai** (bug production)     | Trung bình | CI/CD + automated tests            | Rollback version (ECS task revision) |
| **Chi phí vượt kiểm soát** (cost spike) | Cao 🔴      | Auto Scaling limit, WAF rate limit | AWS Budgets + Billing alarm          |
| **Hiệu năng khi scale**                 | Trung bình | Cache Redis, CDN CloudFront        | Scale DB / read replica              |
  

### 10. Kết quả kỳ vọng  
- Hạ tầng: Kiến trúc cloud-native (Multi-AZ, Auto Scaling) giúp hệ thống đạt high availability, dễ mở rộng và giảm rủi ro downtime.
- Hiệu quả vận hành: Sử dụng managed services (Fargate, RDS, CloudFront) giúp giảm tải DevOps, tăng tốc phát triển và tối ưu chi phí.
- Tối ưu tài chính: Chi phí thấp nhưng biên lợi nhuận cao, tạo nền tảng tốt để scale mà không làm tăng chi phí tương ứng.

> Xây dựng một nền tảng SaaS ổn định, dễ mở rộng và có khả năng tăng trưởng dài hạn với hiệu quả chi phí cao.