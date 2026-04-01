---
title: "Event 2"
date: 2026-03-20
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
# Bài thu hoạch “Cloud Mastery (#1 AI From Scratch)”

### Mục Đích Của Sự Kiện

- Giới thiệu **Strands Agent**: hỗ trợ build agents với các dòng code đơn giản hơn
- Giới thiệu phương pháp để **LLM prompting** cho ra kết quả chất lượng (Nghệ thuật giao tiếp với AI)
- Chia sẻ **case-study AIoT Project**: Locker Manager (IoT & AWS services)

### Danh Sách Diễn Giả

- **Banh Cam Vinh** -  
- **Nguyen Tuan Thinh** - DevOps Engineer, First Cloud AI Journey
- **Aiden Dinh** - Katalon Operation Engineer

### Nội Dung Nổi Bật

#### Đưa ra bất lợi của kiến trúc ứng dụng cũ

- **Code** để buidling tích hợp một tool/agent **dài** và **khai báo phức tạp** → **Tốn thời gian, dễ sai** 
- **kỹ năng prompt** không chuẩn → Câu trả lời nhận được **không đạt kết quả mong muốn**, **tốn thời gian** prompt lại, **tốn token**
- **Quản lý** việc mượn dụng cụ học tập **bằng sức người** → **Bất tiện** khi quản lý **vắng mặt**, **khó kiểm soát**  

#### Chuyển đổi sang kiến trúc ứng dụng mới

#### 1. Strands Agents
- **Mã nguồn mở giúp building agent với ít dòng code và khai báo đơn giản hơn**
- **Lợi ích**: Dễ sử dụng, công cụ gốc, máy chủ MCP, tích hợp các dịch vụ AWS, khả năng mở rộng, phát triển nhanh 
- **Case study - Customer Support Agent**: Minh họa cách tạo đơn giản một agent (Amazon Bedrock), khai báo và gắn tool (task) vào agent, workflow

#### 2. Enhancing LLM Output Quality
- **Nghệ thuật giao tiếp với AI**: Đưa ra các vấn đề khi prompt và các **Key Components** để có một prompt chuẩn giúp **tối ưu chất lượng** câu trả lời và **chi phí token**
- **Case study - Protimizer**: Giới thiệu **Browser extension (Chrome)** giúp **tối ưu hóa câu prompt** và cho phép **chat vs AI mọi tabs** mà không bị ngắt đoạn cuộc trò chuyện trước đó
- **Dịch vụ AWS nổi bật**: Amazon Cognito, Amazon Bedrock, AWS Lamda, Amazon DynamoDB,..    
 
#### 3. Case study - AIoT Project

- **Mission**: Thiết kế hệ thống quản lý tủ locker tự động
- **Technique**: Sử dụng **Arduino** và các **cảm biến** để theo dõi đồ dùng, kết hợp **AWS IoT Core** và **Rekognition** để kết nối các thiết bị và nhận diện khuôn mặt 
- **Dịch vụ AWS nổi bật**: 
    + **AWS IoT Core** (*máy chủ trung gian cho phép kết nối an toàn giữa các thiết bị IoT và dịch vụ AWS*), 
    + **AWS Rekognition** (*nhận diện khuôn mặt trên các hình ảnh đã chụp so với hình ảnh các thành viên được lưu trong S3 Bucket*)

### Những Gì Học Được
- **Strands Agents**: Giúp đơn giản hóa việc phát triển, dễ tích hợp với các dịch vụ cloud (AWS) và tăng khả năng mở rộng so với kiến trúc truyền thống (phức tạp, khó mở rộng, tốn thời gian).

- **Tư duy prompt engineering**: Thiết kế prompt hiệu quả để tối ưu chất lượng đầu ra và chi phí sử dụng LLM. 

- **Case study**: Biết được các áp dụng các dịch vụ AWS vào hệ thống giúp tối ưu hóa năng suất hoạt động và tự động.

### Trải nghiệm trong event

Tham gia workshop **“Cloud Mastery”** là một trải nghiệm rất bổ ích, giúp mình có cái nhìn toàn diện về **cách hiện đại hóa ứng dụng và cơ sở dữ liệu bằng các phương pháp, dịch vụ và công cụ hiện đại**. Qua các case study thực tế, hiểu rõ hơn cách ứng dụng và triển khai hệ thống lên cloud, hình dung rõ hơn về kiến trúc hạ tầng của AWS.

#### Kết nối và trao đổi
- Workshop tạo cơ hội trao đổi trực tiếp với các diễn giả, chuyên gia, tech team, giúp kết nối, mở rộng cộng đồng để học hỏi được thêm nhiều kiến thức bổ ích.


#### Một số hình ảnh khi tham gia sự kiện

![](/images/3-Events/event2.1.jpg)
![](/images/3-Events/event2.2.jpg)

> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp mình thay đổi cách tư duy về thiết kế ứng dụng, cách áp dụng những gì đã học vào các dự án hiệu quả hơn.
