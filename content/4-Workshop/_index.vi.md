---
title: "Workshop"
date: "2000-01-01"
weight: 05
chapter: false
pre: " <b> 5. </b> "
---

# Thiết lập hệ thống phản hồi sự cố tự động và pháp y số trên AWS

#### Tổng quan

Hướng dẫn này cung cấp quy trình từng bước hoàn chỉnh để triển khai hệ thống phản hồi sự cố và điều tra số (forensics) tự động của chúng tôi trên AWS. Hệ thống này tận dụng **CloudTrail**, **GuardDuty**, **VPC Flow Logs**, **Kinesis Firehose**, **Glue**, **Athena**, và **Lambda** functions được điều phối bởi **AWS Step Functions** để tự động phát hiện, phân tích và cách ly các tài nguyên bị xâm phạm như EC2 instances và IAM users. Khả năng điều tra log sâu hơn được bổ sung bằng cách thiết lập **Security Dashboard** lưu trữ trên S3 và truy cập qua **CloudFront** và **Cognito**, truy vấn log sử dụng **API Gateway** và **Lambda**.

#### Nội dung
1. [Tổng quan](5.1-Workshop-overview/)
2. [Điều kiện tiên quyết](5.2-Prerequiste/)
3. [Giai đoạn 1: Thiết lập nền tảng](5.3-Foundation-Setup/)
4. [Giai đoạn 2: Thiết lập giám sát](5.4-Monitoring-Setup/)
5. [Giai đoạn 3: Thiết lập xử lý](5.5-Processing-Setup/)
6. [Giai đoạn 4: Thiết lập tự động hóa](5.6-Automation-Setup/)
7. [Giai đoạn 5: Thiết lập Dashboard](5.7-Dashboard-Setup/)
8. [Kiểm tra](5.8-Verify-Setup/)
9. [Sử dụng CDK](5.9-Use-CDK/)
10. [Dọn dẹp](5.10-Cleanup/)
11. [Phụ lục](5.11-Appendices/)