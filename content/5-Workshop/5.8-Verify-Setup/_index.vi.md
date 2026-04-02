---
title : "Kiểm tra thiết lập"
date: "2000-01-01"
weight : 08
chapter : false
pre : " <b> 5.8. </b> "
---

Sau tất cả các giai đoạn thiết lập, vui lòng tham khảo checklist để đảm bảo việc tạo tài nguyên đã hoàn tất.

## Xác minh thiết lập

**Checklist xác minh hoàn thành:**
- **Incident Response and Forensics:**
  - ✅ **S3 Buckets**: Tất cả 5 buckets đã được tạo với versioning/encryption
  - ✅ **IAM Roles**: Tất cả 17 roles với đúng policies
  - ✅ **CloudTrail**: Logging đã được bật
  - ✅ **GuardDuty**: Đã bật với S3 export
  - ✅ **VPC Flow Logs**: Đang hoạt động (Active)
  - ✅ **Lambda Functions**: Tất cả 9 functions đã deploy
  - ✅ **Firehose Streams**: Tất cả 3 streams đang hoạt động
  - ✅ **Glue Tables**: Tất cả 4 tables đã được tạo
  - ✅ **S3 Events**: Tất cả 4 triggers đã được cấu hình
  - ✅ **SNS Topic**: Đã tạo với subscription
  - ✅ **Step Functions**: Đang hoạt động (Active)
  - ✅ **EventBridge Rule**: Đã bật với 2 targets

- **Security Dashboard:**
  - ✅ **S3 Buckets**: Bucket đã được tạo với file dashboard được lưu trữ và bật hosting
  - ✅ **Query Lambda**: Lambda đã được tạo với các roles thích hợp
  - ✅ **API Gateway**: API Gateway đã được tạo với đúng API và tài nguyên
  - ✅ **CloudFront**: Distribution đã được tạo với API và S3 origins đã cấu hình
  - ✅ **Cognito**: Đã liên kết với CloudFront distribution và tạo user trong user pool


**Kiểm tra đầu cuối (End-to-End Test)**

1.  **Tạo mẫu các phát hiện GuardDuty**: 
   1.1 GuardDuty Console → Settings → Generate sample findings (200+ findings)
   hoặc
   1.2 Kích hoạt một finding đơn lẻ qua CloudShell (Detector Id nằm trong GuardDuty Console → Settings )
```bash
   aws guardduty create-sample-findings --detector-id [$dectector-id] --finding-types "Recon:EC2/PortProbeUnprotectedPort"

```
2.  **Giám sát workflow**: Kiểm tra EventBridge, SNS, Step Functions, Lambda logs
3.  **Xác minh cảnh báo**: Kiểm tra email và Slack
4.  **Truy vấn dữ liệu trong Athena**: