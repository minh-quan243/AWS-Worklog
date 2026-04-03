---
title : "Tổng quan"
date: "2000-01-01" 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Các thành phần hệ thống
+ **Phản hồi sự cố và Điều tra số tự động (Auto Incident Response and Forensics)** là một kiến trúc sử dụng các dịch vụ tự động hóa để thu thập, xử lý và tự động phản hồi các phát hiện bảo mật, giảm thiểu thời gian cần thiết cho sự can thiệp của con người và hỗ trợ nhân viên bảo mật trong việc trực quan hóa và phân tích log.
+ Hệ thống này được xây dựng dựa trên **AWS Security Services** (CloudTrail, GuardDuty, VPC Flow Logs, CloudWatch) đưa dữ liệu vào một **Data Lake tập trung (S3/Glue/Athena)** để phân tích.
+ Tự động hóa cốt lõi được điều khiển bởi **AWS EventBridge** rules kích hoạt **AWS Step Functions** workflows, sau đó thực thi **AWS Lambda** functions để thực hiện các hành động cách ly và cảnh báo.

![Workshop Architecture](/images/2-Proposal/AWSWorkshopArchitecture-Final.png)
_Ảnh kiến trúc của Workshop_

#### Tổng quan về Workshop
Trong workshop này, bạn sẽ triển khai một hệ thống đa giai đoạn để đạt được tự động hóa bảo mật từ đầu đến cuối. Bao gồm:
+ **Thiết lập nền tảng (Foundation Setup)**: Tạo các S3 buckets và IAM roles chuyên dụng để hỗ trợ tất cả các dịch vụ.
+ **Thiết lập giám sát (Monitoring Setup)**: Kích hoạt và cấu hình các log bảo mật chính (CloudTrail, GuardDuty, VPC Flow Logs) để chuyển dữ liệu đến điểm thu thập log trung tâm.
+ **Thiết lập xử lý (Processing Setup)**: Triển khai Kinesis Firehose, Lambda ETLs, và Glue/Athena tables để chuyển đổi log thô thành một security data lake dễ dàng truy vấn.
+ **Thiết lập tự động hóa (Automation Setup)**: Tạo **Isolation Security Group**, **SNS Topic**, **Incident Response Lambda Functions**, và **Step Functions State Machine** thực thi các hành động cách ly tự động khi GuardDuty phát hiện các vấn đề.
+ **Thiết lập Dashboard (Dashboard Setup)**: Lưu trữ một **giao diện web tĩnh dựa trên S3** an toàn được tăng tốc bởi **CloudFront** và bảo vệ bởi **Cognito** để cung cấp cho các nhà phân tích khả năng trực quan hóa thời gian thực của **dữ liệu điều tra (forensic data)** và khả năng truy vấn trực tiếp qua **API Gateway**.
