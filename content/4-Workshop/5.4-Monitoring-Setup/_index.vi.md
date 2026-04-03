---
title : "Thiết lập giám sát"
date: "2000-01-01"
weight : 04
chapter : false
pre : " <b> 5.4. </b> "
---

Giai đoạn Thiết lập giám sát này kích hoạt và cấu hình ba nguồn log cốt lõi để phát hiện mối đe dọa. Giai đoạn này bao gồm việc bật CloudTrail cho các sự kiện quản lý và dữ liệu toàn diện, kích hoạt GuardDuty để xuất các phát hiện bảo mật sang S3 bucket chính, và thiết lập VPC Flow Logs trên mạng của bạn để gửi tất cả metadata lưu lượng truy cập đến CloudWatch Log Group chuyên dụng. Điều này đảm bảo luồng dữ liệu log liên tục, tập trung luôn sẵn sàng cho việc xử lý và phản hồi tự động.


## Tạo CloudWatch Log Group

1.  **Mở CloudWatch Console** → **Log Management** → **Create log group**
![alt text](</images/5-Workshop/Workshop pic/21 Open CloudWatch Console → Log groups → Create log group 5.4.png>)
2.  **Cấu hình**:

      - **Log group name**: `/aws/incident-response/centralized-logs`
      - **Retention**: 90 ngày
      - **KMS key**: None
3.  **Nhấn "Create"**
-----

## Bật AWS CloudTrail

1.  **Mở CloudTrail Console** → **Trail** → **Create trail**
![alt text](</images/5-Workshop/Workshop pic/22 Open CloudTrail Console 5.4.png>)
2.  **Thuộc tính Trail**:

      - **Trail name**: `incident-responses-cloudtrail-ACCOUNT_ID-REGION`
      - **Storage location**: Sử dụng S3 bucket hiện có
      - **S3 bucket**: Chọn `incident-response-log-list-bucket-ACCOUNT_ID-REGION` của bạn
      - **Log file SSE-KMS encryption**: Disable (Tắt)
      - **Log file validation**: Enabled (Bật)
      - Nhấn next

3.  **Chọn log events**:
      - **Events** Chọn **Management events**, **Data events**
      - **Management events**: Tất cả (Read + Write)
      - **Data events**: S3 - Log tất cả events
      - Nhấn next đến bước 4 và **Create Trail**

4.  **Advanced event selectors: Loại trừ log buckets**:
      - **Nhấn vào Trail sau đó cuộn xuống Data Event và nhấn Edit**
![alt text](</images/5-Workshop/Workshop pic/23 Advanced event selectors Exlcude log buckets.png>)
      - **Thiết lập như hình với định dạng dưới đây**:
![alt text](</images/5-Workshop/Workshop pic/24 [Screenshot CloudTrail septup huhu] .png>)

-`arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/`

-`arn:aws:s3:::processed-guardduty-findings-ACCOUNT_ID-REGION/` 

-`arn:aws:s3:::processed-cloudtrail-logs-ACCOUNT_ID-REGION` 

-`arn:aws:s3:::athena-query-results-ACCOUNT_ID-REGION/`   

-`arn:aws:s3:::processed-cloudwatch-logs-ACCOUNT_ID-REGION/`

5.  **Lưu thay đổi**

-----

## Bật Amazon GuardDuty

1.  **Mở GuardDuty Console** → **Get Started** → **Enable GuardDuty**

2.  **Cấu hình cài đặt**:

      - **Finding export frequency**: Update CWE và S3 mỗi 15 phút
      - **S3 export**: `incident-response-log-list-bucket-ACCOUNT_ID-REGION`
      - **KMS encryption**: Chọn hoặc tạo KMS key

-----

## Bật VPC Flow Logs

1.  **Mở VPC Console** → **Your VPCs** → Chọn VPC của bạn

2.  **Actions** → **Create flow log**

3.  **Cấu hình**:

      - **Filter**: All (Tất cả)
      - **Aggregation interval**: 10 phút
      - **Destination**: CloudWatch Logs
      - **Log group**: `/aws/incident-response/centralized-logs`
      - **IAM role**: `FlowLogsIAMRole`
      - **Log format**: Default (Mặc định)
  
4.  **Tạo flow log**

-----

## Bật VPC DNS Query Logging

### Cấu hình Resolver Query Logging

1.  **Mở Amazon Route 53 Console**.
   
2.  Ở thanh điều hướng bên trái, chọn **VPC Resolver** -> **Query logging**.

3.  Nhấn **"Configure query logging"**.

4.  **Cấu hình**:

    * **Name**: Nhập tên mô tả, ví dụ: `IR-DNS-Query-Log-Config`.
    **Destination for query logs**: CloudWatch Logs log group
    * **Log group**: Chọn **"Existing log group"** và chọn:
        * **`/aws/incident-response/centralized-logs`**

5.  Nhấn **"Configure query logging"**.

-----
