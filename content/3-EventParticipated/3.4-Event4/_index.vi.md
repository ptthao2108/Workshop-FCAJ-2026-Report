---
title: "Event 4"
date: 2026-04-04
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Bài thu hoạch "Cloud Native & Infrastructure - Kubernetes, Elixir & IaC"

### Mục Đích Của Sự Kiện

- Giới thiệu những khái niệm nền tảng trong hệ sinh thái cloud-native hiện đại
- Cung cấp góc nhìn thực tế về Kubernetes, DevOps và Infrastructure as Code
- Chia sẻ tư duy thiết kế hệ thống có khả năng mở rộng và chịu lỗi tốt
- Giúp người tham dự hình dung rõ hơn cách một hệ thống production được xây dựng và vận hành

### Danh Sách Diễn Giả

- **Bao Huynh** - Junior Cloud Native Developer, Endava Vietnam; Founder / Head Lab, ITea Lab
- **Nguyen Ta Minh Triet** - R&D Member at ITea Lab, Swinburne Vietnam; SAP Developer Intern, Bosch GSV
- **Thinh Nguyen** - FCAJ Cloud Engineer Trainee

### Nội Dung Nổi Bật

#### 1. Kubernetes và Amazon EKS

**Kubernetes là gì**

- Kubernetes là nền tảng điều phối container
- Hỗ trợ tự động triển khai ứng dụng
- Hỗ trợ mở rộng tài nguyên theo nhu cầu
- Tăng khả năng self-healing cho hệ thống
- Hỗ trợ expose service và cân bằng tải

Có thể xem Kubernetes như một lớp điều phối dành cho các hệ thống phân tán.

**Amazon EKS**

- EKS là dịch vụ Kubernetes được AWS quản lý
- AWS phụ trách control plane như API server, etcd và cơ chế đảm bảo tính sẵn sàng
- Người dùng tập trung quản lý worker nodes, ứng dụng và các workload đang chạy

**Vì sao sử dụng EKS**

- Giảm đáng kể công sức so với tự dựng Kubernetes từ đầu
- Tích hợp thuận tiện với các dịch vụ sẵn có của AWS
- Giúp rút ngắn thời gian đưa hệ thống lên production

**Vì sao vẫn dùng Kubernetes thuần**

- Hạn chế phụ thuộc vào một nhà cung cấp cloud
- Phù hợp với mô hình multi-cloud hoặc hybrid
- Dễ tinh chỉnh sâu hơn ở mức network, scheduler hoặc hành vi cluster

**Kiến trúc EKS tổng quan**

- Control plane do AWS vận hành
- Worker nodes có thể chạy trên EC2 hoặc Fargate
- Hệ thống mạng được tổ chức trong VPC
- Load balancing có thể đi cùng ALB hoặc NLB

**Insight chính**

- EKS phù hợp khi cần triển khai nhanh và giảm gánh nặng vận hành
- Kubernetes thuần phù hợp hơn khi đội ngũ cần mức linh hoạt và kiểm soát cao
- Bài học quan trọng là chọn trade-off phù hợp với bối cảnh hệ thống, không phải chọn công cụ theo cảm tính

#### 2. Elixir, BEAM và Fault-Tolerance

**Elixir và BEAM**

- Elixir là ngôn ngữ lập trình theo hướng functional
- Ngôn ngữ này chạy trên BEAM, runtime nổi tiếng của hệ sinh thái Erlang

**Process model**

- Sử dụng lightweight process thay cho thread truyền thống
- Các process không dùng chung bộ nhớ
- Giao tiếp với nhau thông qua message passing

Lợi ích:

- Hạn chế nhu cầu dùng lock
- Giảm nguy cơ race condition
- Phù hợp với các hệ thống có mức độ đồng thời cao

**Fault-tolerance**

- Triết lý trung tâm là "Let it crash"
- Thay vì cố bao phủ mọi lỗi, hệ thống được thiết kế để chấp nhận lỗi cục bộ và phục hồi an toàn

**OTP**

- Cung cấp supervisor tree
- Cung cấp GenServer
- Cung cấp restart strategy

Những thành phần này giúp khả năng phục hồi trở thành một phần của kiến trúc hệ thống.

**Process supervision**

- Worker chịu trách nhiệm thực hiện tác vụ
- Supervisor theo dõi trạng thái của worker
- Khi worker dừng bất thường, supervisor có thể khởi động lại tiến trình đó

**Hot code upgrade**

- Elixir hỗ trợ cập nhật code với rất ít hoặc gần như không có downtime
- Trong thực tế hiện nay, ý tưởng này thường đi cùng rolling update hoặc blue-green deployment trên Kubernetes

**DevOps và Elixir**

Các công cụ được nhắc đến:

- Mix để build dự án
- Hex để quản lý package
- ExUnit để kiểm thử
- Observer để theo dõi runtime

**Insight chính**

- Một hệ thống ổn định không chỉ dựa vào việc né lỗi mà còn dựa vào khả năng phục hồi sau lỗi
- Elixir thể hiện rõ tư duy resilience-first, khác với cách tiếp cận defensive programming quen thuộc ở nhiều backend truyền thống

#### 3. Infrastructure as Code

**IaC là gì**

- Infrastructure as Code là cách mô tả và quản lý hạ tầng bằng mã nguồn thay vì thao tác thủ công

Lợi ích:

- Tăng khả năng tự động hóa
- Dễ tái tạo môi trường
- Thuận tiện cho version control

**Terraform**

- Hỗ trợ làm việc trong môi trường multi-cloud
- Sử dụng mô hình declarative để mô tả trạng thái hạ tầng mong muốn

Cấu trúc cơ bản thường gồm:

- `main.tf`
- `variables.tf`
- `outputs.tf`
- `providers.tf`

Cấu trúc nâng cao có thể gồm:

- `modules` để tái sử dụng thành phần hạ tầng
- `environment` như dev, staging, prod
- `bootstrap` để thiết lập backend hoặc state ban đầu

**AWS CloudFormation**

- Là công cụ IaC gốc của AWS
- Sử dụng template YAML hoặc JSON
- Cũng đi theo hướng khai báo trạng thái mong muốn

**AWS CDK**

- Cho phép định nghĩa hạ tầng bằng ngôn ngữ lập trình như TypeScript hoặc Python
- Tổ chức theo mô hình App -> Stack -> Construct

**CDK và Terraform**

- CDK thuận lợi với developer làm việc sâu trong hệ sinh thái AWS
- Terraform nổi bật hơn ở bài toán multi-cloud và khả năng tái sử dụng từ hệ sinh thái rộng

**LocalStack**

- Mô phỏng các dịch vụ AWS ngay trên môi trường local
- Giúp kiểm thử thuận tiện hơn trước khi sử dụng tài nguyên cloud thật

**Insight chính**

- Bản thân công cụ không phải yếu tố quyết định tất cả
- Hiệu quả lâu dài phụ thuộc nhiều hơn vào structure, tính nhất quán và kỷ luật triển khai của đội ngũ

### Những Gì Học Được

#### Tư Duy Công Nghệ

- Hiểu rõ hơn cách nhìn ở mức hệ thống đối với kiến trúc cloud-native
- Nhận ra quyết định kỹ thuật luôn gắn với trade-off thay vì đáp án tuyệt đối
- Tiếp cận rõ hơn tư duy thiết kế hệ thống theo hướng resilience ngay từ đầu

#### Kiến Thức Kỹ Thuật

Làm quen với:

- Kubernetes và Amazon EKS
- Fault-tolerant system với Elixir và BEAM
- Infrastructure as Code

Hiểu sơ bộ cách:

- Tổ chức và vận hành dịch vụ trong môi trường gần với production
- Quản lý hạ tầng bằng mã nguồn có cấu trúc

#### Ứng Dụng Vào Công Việc

Có thể áp dụng:

- Tư duy IaC vào các tác vụ quản lý hạ tầng
- Khái niệm điều phối container khi làm việc với môi trường cloud

Trong tương lai:

- Xây dựng hệ thống có khả năng mở rộng và bảo trì tốt hơn
- Gắn thực hành Kubernetes và DevOps gần hơn với các dự án thực tế

### Trải nghiệm trong event

Sự kiện mang lại cho em góc nhìn tổng quát hơn về cách một hệ thống cloud-native được hình thành từ cả phía ứng dụng lẫn phía hạ tầng. Thay vì chỉ giới thiệu từng công cụ riêng lẻ, các phần trình bày cho thấy Kubernetes, Elixir và IaC kết nối với nhau như thế nào trong việc xây dựng một nền tảng ổn định và sẵn sàng cho production.

Phần chia sẻ về EKS và tự động hóa hạ tầng là nội dung em thấy thực tế nhất vì nó liên hệ trực tiếp giữa lựa chọn kiến trúc với triển khai và vận hành. Bên cạnh đó, talk về Elixir và BEAM cũng mở rộng cách nhìn của em về failure, nhấn mạnh rằng phục hồi nên được thiết kế sẵn trong hệ thống.

Một vài nội dung vẫn khá chuyên sâu so với mức tiếp cận ban đầu của em, nên hiện tại em mới nắm tốt ở mức khái niệm tổng quan. Dù vậy, sự kiện đã tạo nền tảng tốt để em tiếp tục tìm hiểu sâu hơn sau này.

### Bài học rút ra

- Việc chọn công nghệ nên dựa trên trade-off thực tế thay vì chạy theo xu hướng
- Hệ thống production cần chiến lược phục hồi, không chỉ chiến lược phòng tránh lỗi
- Quản lý hạ tầng bằng code giúp tăng tính nhất quán và khả năng tái sử dụng
- Cloud-native đòi hỏi sự kết hợp giữa tư duy phát triển phần mềm và tư duy vận hành hệ thống

