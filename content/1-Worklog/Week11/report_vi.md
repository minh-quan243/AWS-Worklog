---
title: "Worklog Tuần 11"
date: 2026-03-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Tiếp tục phát triển dự án theo hướng **fullstack application** trên AWS.
* Triển khai hạ tầng thực tế với EC2 và cấu hình mạng phù hợp.
* Tập trung xây dựng **backend service** và bắt đầu phát triển **frontend**.
* Kết nối các thành phần để hình thành hệ thống hoàn chỉnh hơn.
* Làm quen với quy trình phát triển ứng dụng thực tế trên cloud.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Triển khai **EC2 instance** <br>&emsp; + Lựa chọn instance phù hợp (t2.micro / free tier) <br>&emsp; + Cấu hình security group (SSH, HTTP) <br> - Thiết lập **VPC và Subnet** <br>&emsp; + Public subnet cho EC2 <br>&emsp; + Route Internet Gateway <br> - Chuẩn bị môi trường <br>&emsp; + Cài đặt Python, pip, virtual environment <br>&emsp; + Thiết lập môi trường chạy ứng dụng | 23/03/2026 | 23/03/2026 |  |
| 3 | - Bắt đầu phát triển **backend bằng Python** <br>&emsp; + Xây dựng cấu trúc project (MVC hoặc modular) <br>&emsp; + Tạo API cơ bản (RESTful) <br> - Phát triển các module xử lý logic <br>&emsp; + Xử lý request/response <br>&emsp; + Kết nối database <br> - Test API bằng Postman | 24/03/2026 | 24/03/2026 |  |
| 4 | - Hoàn thiện backend <br>&emsp; + Xây dựng chức năng **chat cơ bản** <br>&emsp; + Xử lý gửi/nhận message <br> - Kiểm tra kết nối giữa các thành phần <br>&emsp; + API ↔ Database <br>&emsp; + Backend ↔ Client | 25/03/2026 | 25/03/2026 | > |
| 5 | - Mở rộng hệ thống chat <br>&emsp; + Lưu trữ lịch sử trò chuyện <br>&emsp; + Thiết kế schema dữ liệu <br>&emsp; + Tối ưu truy vấn (query performance) <br> - Kiểm tra và tối ưu backend <br>&emsp; + Xử lý lỗi <br>&emsp; + Logging cơ bản | 26/03/2026 | 26/03/2026 |  |
| 6 | - Tìm hiểu **Amazon Cognito** <br>&emsp; + Authentication (Sign up / Login) <br>&emsp; + User Pool <br> - Làm quen với **AWS Amplify** <br>&emsp; + Triển khai frontend <br>&emsp; + Hosting đơn giản <br> - Kết nối frontend với backend <br>&emsp; + Gọi API <br>&emsp; + Test end-to-end flow | 27/03/2026 | 27/03/2026 |  |

---

### Kết quả đạt được tuần 11:

* Triển khai thành công hạ tầng:
  * Thiết lập **EC2, VPC, Subnet, Security Group**

* Xây dựng backend hoàn chỉnh:
  * Phát triển API bằng Python
  * Xử lý logic nghiệp vụ của hệ thống
  * Kết nối database để lưu trữ dữ liệu

* Triển khai hệ thống chat cơ bản:
  * Gửi và nhận message giữa client và server
  * Đảm bảo luồng xử lý dữ liệu hoạt động ổn định

* Mở rộng tính năng:
  * Lưu trữ lịch sử trò chuyện
  * Tối ưu truy vấn và cấu trúc dữ liệu
  * Cải thiện hiệu năng backend

* Bắt đầu phát triển frontend:
  * Sử dụng **Cognito** để quản lý người dùng
  * Triển khai giao diện với **Amplify**
  * Kết nối frontend với backend thông qua API

* Hoàn thiện hệ thống end-to-end:
  * Frontend → API → Backend → Database
  * Người dùng có thể tương tác với hệ thống thực tế