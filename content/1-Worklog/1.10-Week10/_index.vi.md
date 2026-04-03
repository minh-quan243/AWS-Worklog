---
title: "Worklog Tuần 10"
date: 2026-03-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Triển khai một **dự án thực tế hoàn chỉnh trên AWS**.
* Áp dụng các dịch vụ đã học để xây dựng hệ thống end-to-end.
* Làm quen với quy trình:
  * Thiết kế kiến trúc hệ thống
  * Triển khai (deployment)
  * Kiểm thử (testing)
  * Đánh giá và tối ưu
* Bước đầu xây dựng một hệ thống theo hướng **serverless + event-driven**.

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Phân tích yêu cầu dự án <br>&emsp; + Xác định input/output hệ thống <br>&emsp; + Thiết kế luồng xử lý dữ liệu <br> - Đề xuất kiến trúc serverless <br>&emsp; + S3 → Lambda → SQS → Transcribe → DynamoDB <br> - Xác định các dịch vụ AWS cần sử dụng | 16/03/2026 | 16/03/2026 |  |
| 3 | - Thiết lập **IAM** <br>&emsp; + Tạo role cho Lambda và service <br>&emsp; + Áp dụng principle of least privilege <br> - Tạo **S3 bucket** <br>&emsp; + Cấu hình upload file audio <br> - Khởi tạo **DynamoDB** <br>&emsp; + Thiết kế bảng lưu metadata và kết quả | 17/03/2026 | 17/03/2026 |  |
| 4 | - Xây dựng **Lambda function** <br>&emsp; + Xử lý event từ S3 <br>&emsp; + Gửi message vào SQS <br> - Tạo **API Gateway** <br>&emsp; + Expose endpoint để truy vấn kết quả <br> - Thiết lập trigger <br>&emsp; + S3 → Lambda <br>&emsp; + Lambda → SQS | 18/03/2026 | 18/03/2026 |  |
| 5 | - Tích hợp **AWS Transcribe** <br>&emsp; + Xử lý audio thành text <br> - Kết hợp với **SQS** <br>&emsp; + Xử lý message bất đồng bộ <br>&emsp; + Tránh blocking hệ thống <br> - Lưu kết quả vào DynamoDB | 19/03/2026 | 19/03/2026 |  |
| 6 | - Kiểm thử hệ thống <br>&emsp; + Upload file audio → kiểm tra pipeline <br>&emsp; + Kiểm tra kết quả trả về từ API <br> - Debug lỗi và tối ưu <br>&emsp; + Kiểm tra log CloudWatch <br>&emsp; + Điều chỉnh timeout và permission <br> - Đánh giá hiệu năng và hoàn thiện | 20/03/2026 | 20/03/2026 |  |

---

### Kết quả đạt được tuần 10:

* Thiết kế kiến trúc hệ thống:
  * S3 → trigger Lambda
  * Lambda → gửi message qua SQS
  * SQS → xử lý bất đồng bộ
  * Transcribe → xử lý audio
  * DynamoDB → lưu kết quả
  * API Gateway → cung cấp endpoint

* Áp dụng kiến thức bảo mật:
  * Sử dụng IAM role đúng mục đích
  * Không hardcode credentials
  * Kiểm soát quyền truy cập giữa các service

* Xây dựng backend serverless:
  * Viết Lambda function xử lý logic
  * Tạo API để truy vấn dữ liệu

* Hiểu rõ luồng dữ liệu trong hệ thống:
  * Event-driven processing
  * Asynchronous workflow
  * Decoupling giữa các thành phần

* Thực hiện kiểm thử hệ thống:
  * Test với dữ liệu thực tế (audio file)
  * Debug lỗi qua CloudWatch logs
  * Tối ưu cấu hình để hệ thống hoạt động ổn định