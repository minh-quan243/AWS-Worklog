---
title: "Worklog Tuần 3"
date: 2026-01-26
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Mở rộng kiến thức về các dịch vụ nâng cao trong **AWS**.
* Hiểu cách xây dựng hệ thống có khả năng mở rộng (scalable) và tối ưu hiệu năng.
* Làm quen với các dịch vụ liên quan đến phân phối nội dung và định tuyến.
* Tiếp cận các công cụ tự động hóa như **AWS CLI**.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu dịch vụ **Route 53** <br>&emsp; + Khái niệm **DNS** và cách phân giải tên miền <br>&emsp; + Hosted Zone (Public/Private) <br>&emsp; + Các loại Record (A, CNAME, NS, ...) <br>&emsp; + Routing policies (Simple, Weighted, Latency, ...) <br> - Thực hành <br>&emsp; + Tạo Hosted Zone <br>&emsp; + Cấu hình domain trỏ đến tài nguyên (EC2/S3) <br>&emsp; + Kiểm tra khả năng resolve domain | 26/01/2026 | 26/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Ôn tập lại **EC2** và kiến thức nền tảng <br> - Tìm hiểu về **Auto Scaling** <br>&emsp; + Khái niệm scaling (vertical vs horizontal) <br>&emsp; + Launch Template/Configuration <br>&emsp; + Auto Scaling Group (ASG) <br> - Thực hành triển khai Auto Scaling <br>&emsp; + Tạo Launch Template <br>&emsp; + Tạo Auto Scaling Group <br>&emsp; + Cấu hình scaling policy dựa trên CPU <br>&emsp; + Kiểm tra khả năng tự động scale | 27/01/2026 | 27/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Làm quen với **AWS CLI** <br>&emsp; + Cài đặt AWS CLI trên máy local <br>&emsp; + Cấu hình credentials (Access Key, Secret Key) <br>&emsp; + Hiểu cấu trúc lệnh CLI <br> - Thực hành <br>&emsp; + Tạo, liệt kê, xóa tài nguyên (EC2, S3) bằng CLI <br>&emsp; + So sánh thao tác CLI và Console <br>&emsp; + Hiểu lợi ích của automation và scripting | 28/01/2026 | 28/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Nghiên cứu dịch vụ **DynamoDB** <br>&emsp; + Khái niệm NoSQL database <br>&emsp; + Table, Item, Attribute <br>&emsp; + Primary Key (Partition key, Sort key) <br>&emsp; + Ưu điểm về hiệu năng và scalability <br> - Thực hành <br>&emsp; + Tạo bảng DynamoDB <br>&emsp; + Thêm, sửa, xóa dữ liệu <br>&emsp; + Thử truy vấn dữ liệu | 29/01/2026 | 29/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Tìm hiểu dịch vụ **CloudFront** <br>&emsp; + Khái niệm **CDN (Content Delivery Network)** <br>&emsp; + Edge locations và caching <br>&emsp; + Distribution (Web/RTMP) <br> - Thực hành <br>&emsp; + Tạo CloudFront distribution <br>&emsp; + Kết nối với S3/EC2 làm origin <br>&emsp; + Kiểm tra tốc độ truy cập và caching <br>&emsp; + So sánh hiệu năng có và không có CDN | 30/01/2026 | 30/01/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 3:

* Hiểu rõ dịch vụ **Route 53**:
  * Nắm được cơ chế hoạt động của DNS
  * Biết cách cấu hình domain và các loại record cơ bản
  * Hiểu cách định tuyến lưu lượng truy cập

* Áp dụng thành công **Auto Scaling**:
  * Triển khai Auto Scaling Group cho EC2
  * Hiểu cách hệ thống tự động mở rộng/thu hẹp theo tải
  * Nhận thức được vai trò của scaling trong hệ thống thực tế

* Làm quen với **AWS CLI**:
  * Thực hiện quản lý tài nguyên bằng dòng lệnh
  * Hiểu rõ hơn về cách tự động hóa và scripting
  * So sánh được ưu/nhược điểm giữa CLI và Console

* Tiếp cận **DynamoDB**:
  * Hiểu mô hình cơ sở dữ liệu NoSQL
  * Thực hành tạo bảng và thao tác dữ liệu
  * Nhận thấy sự khác biệt giữa NoSQL và relational database

* Nắm được nguyên lý của **CloudFront**:
  * Hiểu cách CDN giúp giảm latency và tăng tốc độ truy cập
  * Triển khai phân phối nội dung thông qua edge locations
  * Thấy rõ lợi ích của caching trong hệ thống web

* Hiểu rõ hơn về kiến trúc hệ thống AWS:
  * Route 53 → Điều hướng domain
  * CloudFront → Tăng tốc nội dung
  * EC2 + Auto Scaling → Xử lý tải linh hoạt
  * DynamoDB → Lưu trữ dữ liệu hiệu năng cao