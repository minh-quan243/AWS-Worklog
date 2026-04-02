---
title : "Thiết lập nền tảng"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.3. </b> "
---

Giai đoạn Thiết lập nền tảng ban đầu này xây dựng các điều kiện tiên quyết cốt lõi cho Hệ thống Phản hồi Sự cố Tự động, tập trung vào việc triển khai lưu trữ chuyên dụng và ủy quyền bảo mật thiết yếu. Điều này bắt buộc phải tạo năm Amazon S3 buckets an toàn để thu thập và xử lý log tập trung, áp dụng Bucket Policy cần thiết để phân phối log an toàn, và định nghĩa 17 IAM roles cùng chính sách cách ly (quarantine policy) để thực thi quyền truy cập đặc quyền tối thiểu (least-privilege access) trên tất cả các dịch vụ AWS được tích hợp.

#### Nội dung

- [Thiết lập Amazon S3 Bucket](5.3.1-set-up-s3-buckets/)
- [Cấu hình S3 Bucket Policy cho Primary Log Bucket](5.3.2-set-up-s3-buckets-policies/)
- [Tạo IAM Roles và Policies](5.3.3-create-iam-roles-and-policies/)