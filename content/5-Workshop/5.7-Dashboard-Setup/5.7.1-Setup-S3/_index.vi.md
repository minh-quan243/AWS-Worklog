---
title : "Cài đặt S3 Bucket cho Dashboard"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.7.1. </b> "
---
Trong hướng dẫn này, bạn sẽ cài đặt một S3 để chứa các file web và folder.
**Quan trọng**: Thay thế `ACCOUNT_ID` bằng AWS Account ID của bạn và `REGION` bằng region mục tiêu (ví dụ: us-east-1) trong tất cả tên bucket.

### Tên Bucket

**static-dashboard-bucket-ACCOUNT_ID-REGION** - Lưu trữ các file web và folder đã build

### Hướng dẫn tạo Bucket

1. **Mở Amazon S3 Console**
   - Điều hướng tới https://console.aws.amazon.com/s3/
   - Hoặc: AWS Management Console → Services → S3

   ![Screenshot: AWS S3 Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/s3_page.png)

2. **Nhấn vào "Create bucket"**

   ![Screenshot: Create Bucket Button](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/S3_create.png)

3. **Cài đặt tạo Bucket**:
   - Giữ các cài đặt như mặc định:
     - Bucket name: Nhập `static-dashboard-bucket-ACCOUNT_ID-REGION`
       -  Ví dụ: `static-dashboard-bucket-123456789012-us-east-1`
     - Ownership: **ACLs disabled**
     - Block Public Access: **Block all public access**
     - Bucket versioning: **Disable**
     - Tags (Tùy chọn): **Thêm nếu bạn muốn**
     - Encryption: **SSE-S3**
     - Bucket key: **Enable**
     - Nhấn **Create bucket**

4. **Xác minh tạo bucket**:
    - Bạn sẽ thấy một thông báo thành công
    - Bucket sẽ xuất hiện trong danh sách S3 bucket của bạn

    ![Screenshot: S3 Console showing newly created bucket](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/S3_create_result.png)

5. **Tải lên files và folder**:
    - Truy cập [Github](https://github.com/LGK2005/Dashboard-Content) để lấy nội dung web và tải lên S3

    ![Screenshot: Upload web content to S3](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/s3_upload.png)