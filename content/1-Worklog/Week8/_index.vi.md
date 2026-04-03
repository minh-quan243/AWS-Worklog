---
title: "Worklog Tuần 8"
date: 2026-03-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tiếp cận các dịch vụ **bảo mật nâng cao trong AWS**.
* Hiểu cách bảo vệ hệ thống, dữ liệu và ứng dụng trên môi trường cloud.
* Làm quen với các công cụ kiểm soát truy cập, mã hóa và giám sát bảo mật.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu **AWS Security Hub** <br>&emsp; + Tổng quan về security compliance <br>&emsp; + Tích hợp với các dịch vụ bảo mật khác <br>&emsp; + Chuẩn bảo mật (CIS, AWS Foundational Security Best Practices) <br> - Khám phá **VPC Endpoint (S3 Gateway Endpoint)** <br>&emsp; + Private access tới S3 không qua Internet <br> - Thực hành <br>&emsp; + Bật Security Hub <br>&emsp; + Cấu hình VPC Endpoint cho S3 <br>&emsp; + Kiểm tra truy cập private | 02/03/2026 | 02/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Nghiên cứu **AWS WAF (Web Application Firewall)** <br>&emsp; + Khái niệm bảo vệ ứng dụng web <br>&emsp; + Web ACL và Rule <br>&emsp; + Các loại rule (IP match, rate limit, managed rules) <br> - Thực hành <br>&emsp; + Tạo Web ACL <br>&emsp; + Áp dụng rule để chặn IP hoặc request bất thường <br>&emsp; + Kết hợp với CloudFront/ALB | 03/03/2026 | 03/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Tìm hiểu **AWS KMS (Key Management Service)** <br>&emsp; + Khái niệm encryption (at rest, in transit) <br>&emsp; + Customer Managed Key vs AWS Managed Key <br>&emsp; + Key rotation <br> - Thực hành <br>&emsp; + Tạo KMS key <br>&emsp; + Áp dụng mã hóa cho S3 hoặc EBS <br>&emsp; + Kiểm tra quyền truy cập key | 04/03/2026 | 04/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Khám phá **AWS Secrets Manager** <br>&emsp; + Quản lý credentials (DB password, API key) <br>&emsp; + Rotation tự động <br> - Tìm hiểu **AWS Firewall Manager** <br>&emsp; + Quản lý policy bảo mật tập trung <br> - Thực hành <br>&emsp; + Lưu trữ secret <br>&emsp; + Truy xuất secret từ ứng dụng <br>&emsp; + Cấu hình policy cơ bản | 05/03/2026 | 05/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Tìm hiểu **Amazon Cognito** <br>&emsp; + Authentication vs Authorization <br>&emsp; + User Pool và Identity Pool <br>&emsp; + JWT token <br> - Thực hành <br>&emsp; + Tạo User Pool <br>&emsp; + Đăng ký và đăng nhập user <br>&emsp; + Test xác thực cơ bản cho ứng dụng | 06/03/2026 | 06/03/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 8:

* Hiểu rõ các công cụ bảo mật tổng thể:
  * Sử dụng **Security Hub** để giám sát và đánh giá trạng thái bảo mật
  * Áp dụng chuẩn bảo mật và phát hiện các vấn đề trong hệ thống

* Nắm được cách thiết lập truy cập private:
  * Sử dụng **VPC Endpoint** để truy cập S3 nội bộ
  * Giảm thiểu rủi ro khi expose tài nguyên ra Internet

* Làm quen với việc bảo vệ ứng dụng:
  * Cấu hình **AWS WAF** để lọc request
  * Hiểu cách giảm thiểu các tấn công phổ biến (DDoS, injection, brute force)

* Nắm chắc cơ chế mã hóa dữ liệu:
  * Sử dụng **KMS** để quản lý key
  * Áp dụng encryption cho dữ liệu lưu trữ
  * Hiểu vai trò của encryption trong bảo mật hệ thống

* Quản lý thông tin nhạy cảm:
  * Sử dụng **Secrets Manager** để lưu credentials
  * Áp dụng nguyên tắc không hardcode secret
  * Hiểu cách rotation giúp tăng bảo mật

* Tiếp cận xác thực người dùng:
  * Sử dụng **Cognito** để quản lý user
  * Hiểu quy trình authentication và authorization
  * Làm quen với token-based authentication

* Hiểu rõ kiến trúc bảo mật AWS:
  * IAM → Identity control
  * WAF → Application protection
  * KMS → Encryption
  * Secrets Manager → Secret management
  * Cognito → User authentication