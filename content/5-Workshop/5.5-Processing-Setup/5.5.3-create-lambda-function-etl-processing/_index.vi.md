---
title : "Tạo Lambda Function - Xử lý ETL"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.5.3. </b> "
---


## Tạo Lambda Functions - Xử lý ETL

Trong phần này, bạn sẽ tạo 5 Lambda functions để xử lý logs và gửi chúng đến Kinesis Firehose hoặc S3.

### incident-response-cloudtrail-etl

  - **Runtime**: Python 3.12
  - **Handler**: `CloudTrailETL.lambda_handler`
  - **Role**: `CloudTrailETLLambdaServiceRole`
  - **Timeout**: 300s, Memory: 128MB
  - **Env**: `FIREHOSE_STREAM_NAME=cloudtrail-firehose-stream`
  - **Code**: [cloudtrail-etl](../../5.11-Appendices/5.11.1-cloudtrail-etl)

### incident-response-guardduty-etl

  - **Runtime**: Python 3.12
  - **Handler**: `guardduty_etl.lambda_handler`
  - **Role**: `GuardDutyETLLambdaServiceRole`
  - **Timeout**: 300s, Memory: 128MB
  - **Env**: `DESTINATION_BUCKET`, `S3_LOCATION_GUARDDUTY`, `DATABASE_NAME`, `TABLE_NAME_GUARDDUTY`
  - **Code**: [guardduty-etl](../../5.11-Appendices/5.11.2-guardduty-etl)

### cloudwatch-etl-lambda

  - **Runtime**: Python 3.12
  - **Handler**: `cloudwatch_etl.lambda_handler`
  - **Role**: `CloudWatchETLLambdaServiceRole`
  - **Env**: `FIREHOSE_STREAM_NAME=vpc-dns-firehose-stream`
  - **Code**: [cloudwatch-etl](../../5.11-Appendices/5.11.3-cloudwatch-etl)

### cloudwatch-eni-etl-lambda

  - **Runtime**: Python 3.12
  - **Handler**: `cloudwatch_eni_etl.lambda_handler`
  - **Role**: `CloudWatchENIETLLambdaServiceRole`
  - **Env**: `FIREHOSE_STREAM_NAME=vpc-flow-firehose-stream`
  - **Code**: [cloudwatch-eni-etl](../../5.11-Appendices/5.11.4-cloudwatch-eni-etl)

### cloudwatch-export-lambda

  - **Runtime**: Python 3.12
  - **Handler**: `cloudwatch_autoexport.lambda_handler`
  - **Role**: `CloudWatchExportLambdaServiceRole`
  - **Env**: `DESTINATION_BUCKET=incident-response-log-list-bucket-ACCOUNT_ID-REGION`
  - **Code**: [cloudwatch-autoexport](../../5.11-Appendices/5.11.5-cloudwatch-autoexport)

## Cấu hình CloudWatch Logs Subscription Filter

### Cấu hình Subscription Filter

1.  **Mở CloudWatch Console**.
2.  Ở ngăn điều hướng bên trái, chọn **Log Management**.

3.  Nhấn vào centralized log group: **`/aws/incident-response/centralized-logs`**.

4.  **Tạo Subscription Filter**:
    * Nhấn vào tab **"Subscription filters"**.
    * Nhấn **"Create Lambda subscription filter"**.

5.  **Cấu hình Destination**:
    * **Destination Lambda function**: Chọn function **`cloudwatch-export-lambda`**.
    * **Log format**: Chọn **"Other"**. (Điều này đảm bảo dữ liệu log thô được chuyển đi hiệu quả để Lambda xử lý).

6.  **Cấu hình Log Format và Filter**:
    * **Subscription filter name**: Nhập tên mô tả, ví dụ, `VPC-Log-Export-Filter`.
    * **Filter pattern**: Để trống trường này **blank**. (Đảm bảo tất cả logs trong group đều được xử lý).

7.  Nhấn **"Start streaming"**.
   
## Cấu hình S3 Event Notifications

S3 Console → `incident-response-log-list-bucket-ACCOUNT_ID-REGION` → Properties → Event notifications

Tạo 4 notifications với Event types/Object creation/✅All object create events:

1.  **CloudTrailETLTrigger**: Prefix `AWSLogs/ACCOUNT_ID/CloudTrail/` → Lambda `incident-response-cloudtrail-etl`
2.  **VPCDNSLogsTrigger**: Prefix `exportedlogs/vpc-dns-logs/` → Lambda `cloudwatch-etl-lambda`
3.  **VPCFlowLogsTrigger**: Prefix `exportedlogs/vpc-flow-logs/` → Lambda `cloudwatch-eni-etl-lambda`
4.  **GuardDutyFindingsTrigger**: Prefix `AWSLogs/ACCOUNT_ID/GuardDuty/` → Lambda `incident-response-guardduty-etl`

-----
