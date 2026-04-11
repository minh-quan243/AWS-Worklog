---
title : "Thiết lập lưu trữ"
date: "2000-01-01"
weight: 4
chapter : false
pre : " <b> 4.4. </b> "
---

Trong phần này, bạn sẽ tạo ba bucket S3 và hai bảng DynamoDB tạo thành lớp dữ liệu liên tục (persistent data layer) của nền tảng Voice Summarizer.

| Tài nguyên | Tên | Mục đích |
|---|---|---|
| Bucket S3 | `one4allthing` (Âm thanh + Bản phiên âm) | Lưu trữ các tệp âm thanh gốc được tải lên và đầu ra của AWS Transcribe |
| Bucket S3 | *(tùy chọn của bạn)* (Vectors) | Lưu trữ các vector nhúng (vector embeddings) và chỉ mục ngữ nghĩa (semantic index) |
| Bảng DynamoDB | `VoiceSummarizerHistory` | Trạng thái xử lý của từng bản ghi âm, tiến độ và bộ nhớ hội thoại |
| Bảng DynamoDB | `User` | Các bản ghi người dùng được cấp phát khi đăng ký Cognito |

#### Nội dung

- [Bucket S3 Âm thanh](4.4.1-s3-audio-bucket/)
- [Bucket S3 Vectors](4.4.2-s3-vectors-bucket/)
- [Các Bảng DynamoDB](4.4.3-dynamodb/)