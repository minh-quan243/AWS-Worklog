---
title : "Thiết lập S3 buckets"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.3.1. </b> "
---

Trong phần này, bạn sẽ tạo 5 S3 buckets phục vụ làm nền tảng cho hệ thống Phản hồi Sự cố Tự động (Auto Incident Response system).

**Quan trọng**: Thay thế `ACCOUNT_ID` bằng AWS Account ID của bạn và `REGION` bằng region mục tiêu của bạn (ví dụ: us-east-1) trong tất cả các tên bucket.

### Tên Bucket

1. **incident-response-log-list-bucket-ACCOUNT_ID-REGION** - Bucket thu thập log chính
2. **processed-cloudtrail-logs-ACCOUNT_ID-REGION** - Lưu trữ CloudTrail logs đã xử lý
3. **athena-query-results-ACCOUNT_ID-REGION** - Lưu trữ kết quả truy vấn Athena
4. **processed-cloudwatch-logs-ACCOUNT_ID-REGION** - Lưu trữ CloudWatch logs đã xử lý
5. **processed-guardduty-findings-ACCOUNT_ID-REGION** - Lưu trữ GuardDuty findings đã xử lý

### Hướng dẫn tạo Bucket

1. **Mở Amazon S3 Console**
   - Truy cập https://console.aws.amazon.com/s3/
   - Hoặc: AWS Management Console → Services → S3

![alt text](</images/5-Workshop/Workshop pic/1 S3 console 5.3.1.png>)

2. **Nhấn vào "Create bucket"**
3. **Cấu hình chung**:
   - **Bucket name**: Nhập `incident-response-log-list-bucket-ACCOUNT_ID-REGION`
     - Ví dụ: `incident-response-log-list-bucket-123456789012-us-east-1`
   - **AWS Region**: Chọn region mục tiêu của bạn (ví dụ: US East (N. Virginia) us-east-1)

![alt text](</images/5-Workshop/Workshop pic/2 S3 name 5.3.1.png>)

4. **Object Ownership**:
   - Giữ mặc định: **ACLs disabled (recommended)**

5. **Cài đặt Block Public Access cho bucket này**:
   - Chọn **"Block all public access"**
   - Đảm bảo tất cả 4 tùy chọn phụ đều được chọn:
     - ✓ Block public access to buckets and objects granted through new access control lists (ACLs)
     - ✓ Block public access to buckets and objects granted through any access control lists (ACLs)
     - ✓ Block public access to buckets and objects granted through new public bucket or access point policies
     - ✓ Block public and cross-account access to buckets and objects through any public bucket or access point policies

6. **Bucket Versioning**:
   - Chọn **"Enable"**

![alt text](</images/5-Workshop/Workshop pic/3 Bucket versioning 5.3.1.png>)

7. **Tags** (tùy chọn):
   - Thêm tags nếu muốn
   - Ví dụ: Key=`Purpose`, Value=`IncidentResponse`

8. **Mã hóa mặc định (Default encryption)**:
   - **Encryption type**: Chọn **"Server-side encryption with Amazon S3 managed keys (SSE-S3)"**
   - **Bucket Key**: Giữ mặc định (**Enabled**)

9. **Cài đặt nâng cao (Advanced settings)**:
   - Giữ nguyên tất cả mặc định

10. **Nhấn "Create bucket"**

11. **Xác minh tạo bucket**:
    - Bạn sẽ thấy thông báo thành công
    - Bucket sẽ xuất hiện trong danh sách S3 buckets của bạn

![alt text](</images/5-Workshop/Workshop pic/5 Bucket create succesfull 5.3.1 .png>)

12. **Lặp lại các bước 2-10 cho 4 buckets còn lại**:
    - `processed-cloudtrail-logs-ACCOUNT_ID-REGION`
    - `athena-query-results-ACCOUNT_ID-REGION`
    - `processed-cloudwatch-logs-ACCOUNT_ID-REGION`
    - `processed-guardduty-findings-ACCOUNT_ID-REGION`

13. **Xác minh tất cả 5 buckets đã được tạo**:
    - Điều hướng đến S3 Console
    - Bạn sẽ thấy tất cả 5 buckets được liệt kê

![alt text](</images/5-Workshop/Workshop pic/6 check for 5 bucket.png>)
