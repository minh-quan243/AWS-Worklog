---
title : "Điều kiện tiên quyết"
date: "2000-01-01" 
weight: 2
chapter : false
pre : " <b> 4.2. </b> "
---

## Các yêu cầu về Truy cập và Thông tin

Trước khi tiến hành thiết lập **Hệ thống Phản hồi Sự cố và Điều tra số AWS Tự động (Automated AWS Incident Response and Forensics System)**, hãy đảm bảo bạn đã thu thập đủ các thông tin và quyền truy cập cần thiết dưới đây.

### 🔑 Truy cập & Định danh

* **Tài khoản AWS với Quyền Quản trị (Administrative Access)**
    * Bạn cần toàn quyền quản trị để tạo tài nguyên trên nhiều dịch vụ AWS.
    * Truy cập vào **AWS Management Console**.
* **AWS Account ID của bạn**
    * Định dạng: số có 12 chữ số (ví dụ: `123456789012`).
    * **Placeholder**: Thay thế `ACCOUNT_ID` trong toàn bộ hướng dẫn.
* **AWS Region Mục tiêu**
    * Chọn region nơi bạn sẽ triển khai hệ thống (ví dụ: `us-east-1`).
    * **Placeholder**: Thay thế `REGION` trong toàn bộ hướng dẫn.
* **VPC ID**
    * Một VPC có ít nhất một subnet được yêu cầu cho VPC Flow Logs.
    * **Placeholder**: Thay thế `YOUR_VPC_ID` trong hướng dẫn.
* **Địa chỉ Email đã xác thực Amazon SES**
    * Cần thiết để gửi và nhận thông báo qua email. Xác thực địa chỉ này trong **SES Console**.
    * **Placeholder**: Thay thế `YOUR_VERIFIED_EMAIL@example.com`.
* **Slack Webhook URL (Tùy chọn)**
    * Nếu bạn muốn nhận thông báo qua Slack, hãy lấy webhook URL từ Slack workspace của bạn.
    * **Placeholder**: Thay thế `YOUR_SLACK_WEBHOOK_URL`.