---
title: "Worklog Tuần 5"
date: 2026-02-09
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Tiếp cận mô hình **serverless** trong AWS và hiểu cách hoạt động của kiến trúc không máy chủ.
* Hiểu cách xây dựng API và kết nối các dịch vụ trong hệ thống phân tán.
* Làm quen với các công cụ quản lý hệ thống và truy cập tài nguyên an toàn.
* Xây dựng tư duy thiết kế hệ thống linh hoạt, dễ mở rộng và giảm chi phí vận hành.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu dịch vụ **AWS Lambda** <br>&emsp; + Khái niệm **serverless computing** <br>&emsp; + Cách Lambda function hoạt động (event-driven) <br>&emsp; + Runtime, handler, trigger <br>&emsp; + Giới hạn và pricing cơ bản <br> - Thực hành <br>&emsp; + Tạo Lambda function bằng Node.js/Python <br>&emsp; + Viết logic xử lý đơn giản (return JSON) <br>&emsp; + Test function trực tiếp trên console | 09/02/2026 | 09/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Nghiên cứu dịch vụ **API Gateway** <br>&emsp; + Khái niệm REST API <br>&emsp; + Endpoint, Method (GET, POST, ...) <br>&emsp; + Integration với Lambda <br> - Thực hành <br>&emsp; + Tạo REST API <br>&emsp; + Kết nối API với Lambda function <br>&emsp; + Deploy API và test bằng Postman/browser | 10/02/2026 | 10/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Thực hiện lab tổng hợp **Lambda + API Gateway** <br>&emsp; + Xây dựng API xử lý request (ví dụ: trả về dữ liệu JSON) <br>&emsp; + Xử lý input/output giữa client và Lambda <br>&emsp; + Debug lỗi khi integration <br> - Hiểu luồng hoạt động: Client → API Gateway → Lambda → Response | 11/02/2026 | 11/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Tìm hiểu **AWS Systems Manager (Session Manager)** <br>&emsp; + Khái niệm quản lý instance không cần SSH <br>&emsp; + Lợi ích về bảo mật (không cần mở port 22) <br>&emsp; + IAM role cho EC2 <br> - Thực hành <br>&emsp; + Cấu hình Session Manager <br>&emsp; + Truy cập EC2 instance thông qua browser/CLI <br>&emsp; + So sánh với phương pháp SSH truyền thống | 12/02/2026 | 12/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Khám phá **AWS CloudFormation** <br>&emsp; + Khái niệm **Infrastructure as Code (IaC)** <br>&emsp; + Template (YAML/JSON) <br>&emsp; + Stack và lifecycle <br> - Thực hành <br>&emsp; + Viết template đơn giản (tạo S3/EC2) <br>&emsp; + Deploy stack từ template <br>&emsp; + Update và delete stack <br>&emsp; + Quan sát quá trình provisioning tự động | 13/02/2026 | 13/02/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 5:

* Hiểu rõ mô hình **serverless**:
  * Nắm được cách hoạt động của **AWS Lambda**
  * Hiểu mô hình event-driven
  * Biết cách triển khai function xử lý logic backend

* Xây dựng được API cơ bản:
  * Sử dụng **API Gateway** để tạo endpoint
  * Kết nối API với Lambda để xử lý request
  * Hiểu rõ luồng request/response trong hệ thống serverless

* Hoàn thành lab thực hành:
  * Triển khai API hoàn chỉnh (client → API → Lambda)
  * Debug và xử lý lỗi integration
  * Tăng khả năng đọc log và troubleshooting

* Làm quen với **Systems Manager**:
  * Truy cập EC2 thông qua **Session Manager**
  * Giảm phụ thuộc vào SSH và key pair
  * Tăng mức độ bảo mật hệ thống

* Tiếp cận **CloudFormation (IaC)**:
  * Hiểu lợi ích của việc triển khai hạ tầng bằng code
  * Viết và deploy template cơ bản
  * Nhận thức được khả năng tự động hóa và tái sử dụng hạ tầng

* Hiểu rõ hơn về kiến trúc hiện đại:
  * API Gateway + Lambda → Serverless backend
  * IAM + Session Manager → Secure access
  * CloudFormation → Automated infrastructure