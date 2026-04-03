---
title: "Worklog Tuần 6"
date: 2026-02-16
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Ôn tập và hệ thống lại toàn bộ kiến thức các dịch vụ **AWS** đã học.
* Tăng cường kỹ năng thực hành thông qua các bài lab tổng hợp.
* Hiểu sâu hơn cách các dịch vụ phối hợp trong một hệ thống thực tế.
* Củng cố tư duy thiết kế hệ thống cloud (network, compute, storage, serverless).

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Ôn tập dịch vụ **EC2** <br>&emsp; + Quy trình tạo và cấu hình instance <br>&emsp; + Key Pair, Security Group, AMI <br>&emsp; + Các lỗi thường gặp khi kết nối <br> - Thực hành <br>&emsp; + Triển khai lại EC2 instance từ đầu <br>&emsp; + Cấu hình truy cập SSH <br>&emsp; + Kiểm tra trạng thái và monitoring cơ bản | 16/02/2026 | 16/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Ôn tập **VPC và Networking** <br>&emsp; + Subnets (Public/Private) <br>&emsp; + Route Tables <br>&emsp; + Internet Gateway, NAT Gateway <br> - Thực hành <br>&emsp; + Xây dựng lại VPC hoàn chỉnh <br>&emsp; + Triển khai EC2 trong các subnet khác nhau <br>&emsp; + Kiểm tra khả năng kết nối giữa các thành phần | 17/02/2026 | 17/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Ôn tập **AWS Lambda** <br>&emsp; + Cấu trúc function <br>&emsp; + Trigger và event <br>&emsp; + Logging và debug <br> - Thực hành <br>&emsp; + Viết Lambda function xử lý logic đơn giản <br>&emsp; + Kết hợp với API Gateway để test <br>&emsp; + Kiểm tra log trên CloudWatch | 18/02/2026 | 18/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Ôn tập **S3 (Simple Storage Service)** <br>&emsp; + Bucket, Object <br>&emsp; + Permission (IAM Policy, Bucket Policy) <br>&emsp; + Public vs Private access <br> - Thực hành <br>&emsp; + Tạo bucket mới <br>&emsp; + Upload và quản lý file <br>&emsp; + Thiết lập quyền truy cập dữ liệu | 19/02/2026 | 19/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Ôn tập **CloudFront** <br>&emsp; + Cơ chế CDN và caching <br>&emsp; + Distribution và Origin <br> - Thực hành <br>&emsp; + Tạo CloudFront distribution <br>&emsp; + Kết nối với S3 làm origin <br>&emsp; + Kiểm tra tốc độ truy cập và caching <br> - Tổng hợp lại toàn bộ kiến thức đã học trong 6 tuần | 20/02/2026 | 20/02/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 6:

* Củng cố kiến thức về **EC2**:
  * Thành thạo quy trình tạo và quản lý instance
  * Hiểu rõ các cấu hình quan trọng (Security Group, Key Pair)
  * Xử lý được các lỗi kết nối cơ bản

* Nắm chắc lại **VPC và Networking**:
  * Hiểu rõ cách thiết kế mạng với Public/Private Subnet
  * Cấu hình routing và kết nối Internet
  * Nhận thức rõ luồng traffic trong hệ thống

* Làm chủ lại **Lambda**:
  * Viết và triển khai function cơ bản
  * Kết hợp với API Gateway để tạo API
  * Sử dụng CloudWatch để debug và theo dõi log

* Tăng kỹ năng với **S3**:
  * Quản lý bucket và object hiệu quả
  * Thiết lập quyền truy cập chính xác
  * Hiểu rõ các vấn đề bảo mật dữ liệu

* Hiểu sâu hơn về **CloudFront**:
  * Triển khai CDN cho nội dung tĩnh
  * Nhận thấy rõ sự cải thiện về tốc độ truy cập
  * Hiểu cơ chế caching và phân phối nội dung

* Tổng hợp kiến thức hệ thống:
  * EC2 → Compute
  * VPC → Networking
  * S3 → Storage
  * Lambda → Serverless
  * CloudFront → CDN