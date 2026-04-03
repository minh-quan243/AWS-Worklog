---
title: "Worklog Tuần 2"
date: 2026-01-19
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Tiếp tục đào sâu các dịch vụ cốt lõi trong **AWS**.
* Hiểu rõ cách triển khai và cấu hình các tài nguyên thực tế trên cloud.
* Nắm được cách các dịch vụ liên kết và hỗ trợ lẫn nhau trong hệ thống.
* Tăng cường kỹ năng thực hành với các dịch vụ phổ biến như **EC2**, **S3**, **CloudWatch**, **RDS**.
* Bước đầu làm quen với việc giám sát và quản lý tài nguyên trên AWS.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu dịch vụ **EC2 (Elastic Compute Cloud)** <br>&emsp; + Khái niệm máy ảo trên cloud <br>&emsp; + Các loại instance (t2, t3, ...) và mục đích sử dụng <br>&emsp; + AMI (Amazon Machine Image) <br>&emsp; + Security Group và cơ chế firewall <br> - Thực hành khởi tạo EC2 instance <br>&emsp; + Lựa chọn instance type phù hợp (Free Tier) <br>&emsp; + Cấu hình key pair để SSH <br>&emsp; + Kết nối instance thông qua SSH | 19/01/2026 | 19/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Ôn tập lại kiến thức **IAM** <br>&emsp; + Review Users, Groups, Policies <br>&emsp; + Kiểm tra lại các nguyên tắc phân quyền <br> - Thực hành lại EC2 <br>&emsp; + Tạo và quản lý nhiều instance <br>&emsp; + Cấu hình Security Group cho từng instance <br> - Kết hợp **IAM với EC2** <br>&emsp; + Sử dụng IAM User để truy cập tài nguyên <br>&emsp; + Hiểu cơ chế phân quyền truy cập instance | 20/01/2026 | 20/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Tìm hiểu dịch vụ **S3 (Simple Storage Service)** <br>&emsp; + Khái niệm Bucket và Object <br>&emsp; + Các storage classes (Standard, IA, Glacier, ...) <br>&emsp; + Quy tắc đặt tên bucket <br> - Thực hành với S3 <br>&emsp; + Tạo bucket <br>&emsp; + Upload, download file <br>&emsp; + Thiết lập quyền truy cập (public/private) <br>&emsp; + Làm quen với giao diện quản lý dữ liệu | 21/01/2026 | 21/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Nghiên cứu dịch vụ **CloudWatch** <br>&emsp; + Khái niệm Monitoring trong cloud <br>&emsp; + Metrics, Logs, Alarms <br>&emsp; + Vai trò của CloudWatch trong vận hành hệ thống <br> - Thực hành <br>&emsp; + Theo dõi trạng thái EC2 instance <br>&emsp; + Quan sát CPU, Network, Disk metrics <br>&emsp; + Tạo cảnh báo (Alarm) cơ bản | 22/01/2026 | 22/01/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Tìm hiểu dịch vụ **RDS (Relational Database Service)** <br>&emsp; + Các loại database hỗ trợ (MySQL, PostgreSQL, ...) <br>&emsp; + Khái niệm managed database <br>&emsp; + Cấu hình cơ bản (instance class, storage, security) <br> - Thực hành triển khai RDS <br>&emsp; + Tạo database instance <br>&emsp; + Cấu hình security group để kết nối <br>&emsp; + Kết nối database từ local/EC2 <br>&emsp; + Kiểm tra hoạt động và truy vấn dữ liệu cơ bản | 23/01/2026 | 23/01/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 2:

* Hiểu rõ hơn về dịch vụ **EC2**:
  * Biết cách lựa chọn instance phù hợp với nhu cầu
  * Nắm được quy trình khởi tạo và cấu hình máy ảo
  * Thành thạo kết nối SSH vào instance

* Củng cố và áp dụng kiến thức **IAM**:
  * Áp dụng phân quyền khi làm việc với EC2
  * Hiểu rõ hơn về bảo mật và kiểm soát truy cập
  * Tránh sử dụng root account trong thao tác thực tế

* Làm chủ các thao tác cơ bản với **S3**:
  * Tạo và quản lý bucket
  * Upload, download và tổ chức dữ liệu
  * Hiểu cơ chế phân quyền truy cập dữ liệu

* Nắm được cách sử dụng **CloudWatch**:
  * Theo dõi trạng thái và hiệu suất tài nguyên
  * Đọc và hiểu các chỉ số metrics cơ bản
  * Thiết lập cảnh báo để giám sát hệ thống

* Tiếp cận dịch vụ **RDS**:
  * Hiểu khái niệm database managed service
  * Triển khai thành công database instance
  * Kết nối và kiểm tra hoạt động database

* Hiểu được sự liên kết giữa các dịch vụ:
  * EC2 + IAM → Quản lý truy cập và bảo mật
  * EC2 + S3 → Lưu trữ và xử lý dữ liệu
  * EC2 + CloudWatch → Giám sát hệ thống
  * EC2 + RDS → Xây dựng hệ thống backend hoàn chỉnh