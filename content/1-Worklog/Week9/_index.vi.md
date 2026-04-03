---
title: "Worklog Tuần 9"
date: 2026-03-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Tiếp cận kiến trúc hệ thống **phân tán (distributed systems)** và **event-driven architecture** trên AWS.
* Hiểu cách xây dựng pipeline **CI/CD** để tự động hóa quá trình phát triển và triển khai.
* Làm quen với **containerization** và vai trò của container trong hệ thống cloud.
* Nâng cao khả năng thiết kế workflow và orchestration giữa các service.
* Chuẩn bị nền tảng cho việc xây dựng hệ thống production-ready.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu hệ thống messaging trong AWS <br>&emsp; + **Amazon SQS (Simple Queue Service)**: xử lý hàng đợi bất đồng bộ <br>&emsp; + **Amazon SNS (Simple Notification Service)**: mô hình publish/subscribe <br>&emsp; + So sánh SQS và SNS <br> - Thực hành <br>&emsp; + Tạo queue và topic <br>&emsp; + Gửi/nhận message giữa các service <br>&emsp; + Mô phỏng luồng xử lý bất đồng bộ giữa producer và consumer | 09/03/2026 | 09/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Làm quen với **Docker** <br>&emsp; + Khái niệm containerization <br>&emsp; + Image và Container <br>&emsp; + Dockerfile và build process <br> - Thực hành <br>&emsp; + Viết Dockerfile đơn giản <br>&emsp; + Build image <br>&emsp; + Chạy container local và kiểm tra ứng dụng | 10/03/2026 | 10/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Nghiên cứu **CI/CD với AWS CodePipeline** <br>&emsp; + Pipeline workflow (Source → Build → Deploy) <br>&emsp; + Tích hợp với GitHub/CodeCommit <br> - Thực hành <br>&emsp; + Tạo pipeline đơn giản <br>&emsp; + Tự động build và deploy ứng dụng khi có thay đổi code <br>&emsp; + Theo dõi pipeline execution | 11/03/2026 | 11/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Tìm hiểu **AWS Storage Gateway** <br>&emsp; + Hybrid storage (kết nối on-premise với cloud) <br>&emsp; + Use case thực tế <br> - Khám phá **DynamoDB nâng cao** <br>&emsp; + Thiết kế bảng theo access pattern <br>&emsp; + Partition key và tối ưu hiệu năng <br> - Thực hành <br>&emsp; + Tạo bảng và test query | 12/03/2026 | 12/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Tìm hiểu **AWS Step Functions** <br>&emsp; + Workflow orchestration <br>&emsp; + State machine (Task, Choice, Parallel) <br> - Thực hành <br>&emsp; + Xây dựng workflow đơn giản <br>&emsp; + Kết hợp với Lambda <br>&emsp; + Theo dõi execution flow | 13/03/2026 | 13/03/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 9:

* Hiểu rõ mô hình **messaging và event-driven**:
  * Sử dụng **SQS** để xử lý queue bất đồng bộ
  * Áp dụng **SNS** cho mô hình publish/subscribe
  * Giảm coupling giữa các service trong hệ thống

* Làm quen với **Docker**:
  * Biết cách build image và chạy container
  * Hiểu vai trò của container trong việc đóng gói ứng dụng
  * Nhận thức được lợi ích khi deploy đa môi trường

* Nắm được quy trình **CI/CD**:
  * Xây dựng pipeline với **CodePipeline**
  * Tự động hóa quá trình build và deploy
  * Giảm thiểu thao tác thủ công trong phát triển phần mềm

* Tiếp cận hệ thống hybrid và database nâng cao:
  * Hiểu cách kết nối hệ thống on-premise với cloud qua **Storage Gateway**
  * Áp dụng tư duy thiết kế NoSQL với **DynamoDB**

* Hiểu cách **orchestration workflow**:
  * Sử dụng **Step Functions** để điều phối các service
  * Thiết kế luồng xử lý logic theo state machine
  * Kết hợp nhiều service thành workflow hoàn chỉnh

* Hiểu rõ kiến trúc hệ thống hiện đại:
  * Messaging → SQS/SNS
  * Container → Docker
  * Automation → CI/CD Pipeline
  * Orchestration → Step Functions