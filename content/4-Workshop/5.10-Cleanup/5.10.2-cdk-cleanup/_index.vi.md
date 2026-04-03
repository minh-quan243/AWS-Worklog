---
title : "Dọn dẹp CDK"
date: "2000-01-01"
weight : 02
chapter : false
pre : " <b> 5.10.2. </b> "
---
##  Clean up (CDK)

Hướng dẫn này đảm bảo bạn hủy bỏ (decommission) chính xác tất cả các tài nguyên được cung cấp bởi AWS CDK stack và dọn dẹp dữ liệu được tạo thủ công để tránh các khoản phí phát sinh.

### Giai đoạn 1: Dọn dẹp dữ liệu thủ công (Trước khi CDK Destroy)

CDK tự động xóa hầu hết các tài nguyên nhưng không xóa nội dung trong S3 buckets. Bạn phải **làm trống nội dung của các buckets này** trước khi chạy lệnh `cdk destroy`.

| Tên Resource | Mục đích | Hành động yêu cầu |
| :--- | :--- | :--- |
| **`incident-response-log-list-bucket`** | Nguồn Log Chính | **Làm trống nội dung** |
| **`processed-cloudwatch-logs`** | ETL Destination | **Làm trống nội dung** |
| **`processed-guardduty-findings`** | ETL Destination | **Làm trống nội dung** |
| **`processed-cloudtrail-logs`** | ETL Destination | **Làm trống nội dung** |
| **`athena-query-results`** | Kết quả truy vấn Athena | **Làm trống nội dung** |
| **`aws-incident-response-automation-dashboard`** | React Dashboard S3 Bucket | **Làm trống nội dung** |

**Hướng dẫn làm trống Buckets:**

1.  Mở **Amazon S3 Console** trong trình duyệt của bạn.
2.  Đối với **mỗi** buckets được liệt kê ở trên (tìm tên dựa trên AWS Account ID và Region của bạn):
    * Nhấn vào tên bucket.
    * Điều hướng đến tab **"Objects"**.
    * Nhấn nút **"Empty"**.
    * Làm theo các lời nhắc để xác nhận xóa vĩnh viễn tất cả các objects.

---

### Giai đoạn 2: CDK Stack Destruction

Bước này sử dụng CDK CLI để phá hủy tất cả các tài nguyên được cung cấp bởi CloudFormation stack.

1.  **Đảm bảo môi trường ảo (Virtual Environment) đang hoạt động**
    * Nếu bạn đã tắt môi trường Python, hãy kích hoạt lại nó (ví dụ: `source .venv/bin/activate`).

2.  **Điều hướng đến Project Root**
    * Đảm bảo bạn đang ở thư mục chính nơi chứa file `cdk.json`.

3.  **Thực thi lệnh Destroy**
    * Chạy lệnh để phá hủy tất cả các stacks đã triển khai. Khi được nhắc, gõ **`y`** để chấp nhận việc xóa.
        ```bash
        $ cdk destroy --all
        ```
---

### Giai đoạn 3: Dọn dẹp sau khi phá hủy

Bước này giải quyết việc dọn dẹp thủ công các tài nguyên còn sót lại.

1.  **Xóa các S3 Buckets còn lại**
    * Lệnh `cdk destroy` sẽ xóa các S3 buckets **trống**. Nếu còn sót lại bucket nào (do kiểm tra cuối cùng hoặc bảo vệ dịch vụ), hãy xóa chúng ngay bây giờ qua S3 Console.

2.  **Vô hiệu hóa Amazon GuardDuty**
    * Vào **GuardDuty Console** → **Settings** → **General**.
    * Xác minh dịch vụ đã bị vô hiệu hóa để đảm bảo ngừng tính phí.

3.  **Xóa Cognito User và Pool**
    * Vào **Cognito Console** → **User pools**.
    * Xóa test user bạn đã tạo.
    * Xóa User Pool đã tạo cho dashboard.

4.  **Xóa SES Identity**
    * Vào **Amazon SES Console** → **Verified Identities**.
    * Xóa sender email identity (`sender_email`) bạn đã xác minh.

5.  **Hủy kích hoạt môi trường ảo**
    * Hủy kích hoạt môi trường ảo Python:
        ```bash
        $ deactivate
        ```