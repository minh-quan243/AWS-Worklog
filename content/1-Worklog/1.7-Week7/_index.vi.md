---
title: "Worklog Tuần 7"
date: 2026-02-23
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tiếp cận các công cụ nâng cao trong **AWS** phục vụ phát triển và quản lý hệ thống.
* Hiểu cách tối ưu tài nguyên và chi phí trong môi trường cloud.
* Nâng cao khả năng giám sát hệ thống và phân tích hoạt động mạng.
* Củng cố kiến thức về bảo mật và kiểm soát truy cập nâng cao.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu **AWS CDK (Cloud Development Kit)** <br>&emsp; + Khái niệm Infrastructure as Code nâng cao <br>&emsp; + CDK Constructs và Stack <br>&emsp; + So sánh CDK với CloudFormation <br> - Thực hành <br>&emsp; + Cài đặt môi trường CDK (Node.js/Python) <br>&emsp; + Viết code tạo resource (S3/EC2) <br>&emsp; + Deploy stack bằng CDK CLI | 23/02/2026 | 23/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Nghiên cứu tối ưu tài nguyên **EC2** <br>&emsp; + Khái niệm **Right-Sizing** <br>&emsp; + Theo dõi CPU, Memory usage <br>&emsp; + Lựa chọn instance phù hợp workload <br> - Thực hành <br>&emsp; + Phân tích metrics trên CloudWatch <br>&emsp; + Điều chỉnh instance type <br>&emsp; + So sánh hiệu năng và chi phí trước/sau tối ưu | 24/02/2026 | 24/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Thực hiện lab về giám sát mạng <br>&emsp; + Tìm hiểu **VPC Flow Logs** <br>&emsp; + Cấu hình logging cho VPC/Subnet <br>&emsp; + Phân tích traffic logs <br> - Thực hành <br>&emsp; + Kiểm tra kết nối giữa EC2 instances <br>&emsp; + Phát hiện và xử lý lỗi network | 25/02/2026 | 25/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Tìm hiểu **Service Quotas** <br>&emsp; + Các giới hạn mặc định của AWS <br>&emsp; + Cách request tăng quota <br> - Nghiên cứu **Cost Management** <br>&emsp; + AWS Cost Explorer <br>&emsp; + Theo dõi và phân tích chi phí <br>&emsp; + Thiết lập cảnh báo ngân sách (Budget) | 26/02/2026 | 26/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Khám phá **IAM nâng cao** <br>&emsp; + IAM Policies nâng cao <br>&emsp; + Condition trong policy <br>&emsp; + Resource-based vs Identity-based policy <br> - Thực hành <br>&emsp; + Viết policy với điều kiện (IP, time, MFA) <br>&emsp; + Kiểm tra và validate quyền truy cập <br>&emsp; + Áp dụng vào tình huống thực tế | 27/02/2026 | 27/02/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 7:

* Làm quen với **AWS CDK**:
  * Hiểu cách định nghĩa hạ tầng bằng code (IaC nâng cao)
  * Triển khai tài nguyên AWS thông qua lập trình
  * Nhận thấy lợi ích của automation trong triển khai hệ thống

* Nắm được cách tối ưu tài nguyên **EC2**:
  * Biết cách đánh giá hiệu năng dựa trên metrics
  * Áp dụng **right-sizing** để giảm chi phí
  * Hiểu mối quan hệ giữa hiệu năng và chi phí

* Thực hành giám sát mạng:
  * Sử dụng **VPC Flow Logs** để theo dõi lưu lượng
  * Phân tích traffic giữa các thành phần trong hệ thống
  * Hỗ trợ troubleshooting các vấn đề network

* Hiểu cách quản lý giới hạn và chi phí:
  * Sử dụng **Cost Explorer** để theo dõi usage
  * Thiết lập cảnh báo ngân sách
  * Nhận thức rõ tầm quan trọng của cost control trong cloud

* Nâng cao kiến thức về **IAM**:
  * Xây dựng policy với điều kiện cụ thể
  * Kiểm soát truy cập chi tiết hơn (IP, thời gian, MFA)
  * Hiểu rõ nguyên tắc bảo mật nâng cao

* Hiểu rõ hơn về vận hành hệ thống:
  * Monitoring → CloudWatch, Flow Logs
  * Cost → Budget, Cost Explorer
  * Security → IAM nâng cao
  * Automation → CDK
