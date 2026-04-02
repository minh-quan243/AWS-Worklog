---
title : "Dọn dẹp thủ công"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.10.1 </b> "
---

## Clean up (Thiết lập cơ sở hạ tầng thủ công)

### Giai đoạn 1: Dọn dẹp Automation và Monitoring

Mục tiêu ở đây là **dừng tất cả các tiến trình đang hoạt động** và xóa các tài nguyên monitoring và automation cốt lõi (EventBridge, Step Functions, SNS, GuardDuty, Flow Logs, CloudTrail).

#### 1. Xóa Incident Response Automation

1.1 **Xóa EventBridge Rule**
* Vào **EventBridge Console** → **Rules**.
* Chọn rule: **`IncidentResponseAlert`**.
* Nhấn **"Delete"**.

1.2 **Xóa Step Functions State Machine**
* Vào **Step Functions Console** → **State Machines**.
* Chọn State Machine: **`IncidentResponseStepFunctions`**.
* Nhấn **"Delete"**.

1.3 **Xóa SNS Topic và Subscription**
* Vào **SNS Console** → **Topics** → **`IncidentResponseAlerts`**.
* Đầu tiên, xóa subscription liên kết với **`ir-alert-dispatch`**.
* Sau đó, xóa chính topic bằng cách nhấn **"Delete topic"**.

1.4 **Xóa GuardDuty Detector**
* Vào **GuardDuty Console** → **Settings** → **General**.
* Nhấn **"Suspend"** để dừng xử lý, sau đó nhấn **"Disable GuardDuty"** (hoặc **"Delete detector"**).

1.5 **Vô hiệu hóa VPC Flow Logs**
* Vào **VPC Console** → **VPC Flow Logs**.
* Chọn flow log đã tạo (liên kết với `YOUR_VPC_ID`).
* Nhấn **"Delete flow log"**.

1.6 **Xóa CloudTrail Trail**
* Vào **CloudTrail Console** → **Trails**.
* Chọn trail: **`incident-responses-cloudtrail-ACCOUNT_ID-REGION`**.
* Nhấn **"Delete"**.

---

### Giai đoạn 2: Dọn dẹp Lambda và Compute

#### 2. Xóa tất cả Lambda Functions (9 Functions)

Vào **Lambda Console** và xóa các functions sau:

* `incident-response-cloudtrail-etl`
* `incident-response-guardduty-etl`
* `cloudwatch-etl-lambda`
* `cloudwatch-eni-etl-lambda`
* `cloudwatch-export-lambda`
* `ir-parse-findings-lambda`
* `ir-isolate-ec2-lambda`
* `ir-quarantine-iam-lambda`
* `ir-alert-dispatch`

#### 3. Xóa Isolation Security Group

* Vào **EC2 Console** → **Security Groups**.
* Tìm và chọn Security Group: **`IR-Isolation-SG`** (sử dụng ID `sg-XXXXXXX`).
* Nhấn **"Delete security group"**.

#### 4. Xóa CloudWatch Log Groups

Vào **CloudWatch Console** → **Log Groups** và xóa:

* Centralized log group: **`/aws/incident-response/centralized-logs`**.
* Bất kỳ **Lambda log groups** nào liên quan đến 9 functions đã xóa (ví dụ: `/aws/lambda/ir-parse-findings-lambda`).

---

### Giai đoạn 3: Dọn dẹp Processing và Data Lake

#### 5. Xóa Kinesis Data Firehose Streams

Vào **Kinesis Console** → **Delivery Streams** và xóa:

* `cloudtrail-firehose-stream`
* `vpc-dns-firehose-stream`
* `vpc-flow-firehose-stream`

#### 6. Xóa AWS Glue Tables và Database

6.1 **Xóa Glue Tables**
* Vào **Glue Console** → **Tables**.
* Chọn và xóa: **`security_logs.processed_cloudtrail`**, **`security_logs.processed_guardduty`**, **`security_logs.vpc_logs`**, và **`security_logs.eni_flow_logs`**.

6.2 **Xóa Glue Database**
* Vào **Glue Console** → **Databases**.
* Chọn database: **`security_logs`** và nhấn **"Delete"**.

#### 7. Xóa IAM Roles và Policies

7.1 **Xóa IAM Policies**
* Vào **IAM Console** → **Policies**.
* Xóa custom managed policy: **`IrQuarantineIAMPolicy`**.
* *Lưu ý: Inline policies được tạo trong quá trình cài đặt sẽ tự động bị xóa khi role tương ứng bị xóa.*

7.2 **Xóa IAM Roles**
* Vào **IAM Console** → **Roles**.
* Xóa 17 roles sau:
    * **Lambda Execution Roles**: `CloudTrailETLLambdaServiceRole`, `GuardDutyETLLambdaServiceRole`, `CloudWatchETLLambdaServiceRole`, `CloudWatchENIETLLambdaServiceRole`, `CloudWatchExportLambdaServiceRole`, `ParseFindingsLambdaServiceRole`, `IsolateEC2LambdaServiceRole`, `QuarantineIAMLambdaServiceRole`, `AlertDispatchLambdaServiceRole`.
    * **Service Roles**: `CloudTrailFirehoseRole`, `CloudWatchFirehoseRole`, `StepFunctionsRole`, `IncidentResponseStepFunctionsEventRole`, `FlowLogsIAMRole`, `GlueCloudWatchRole`.

---

### Giai đoạn 4: Dọn dẹp S3 Bucket (Xóa dữ liệu)

#### 8. Làm trống và Xóa S3 Buckets

Đây là **bước cuối cùng** để đảm bảo tất cả các khoản phí lưu trữ được dừng lại.

| Bucket Name | Mục đích |
| :--- | :--- |
| **`incident-response-log-list-bucket-ACCOUNT_ID-REGION`** | Nguồn Log Chính (CloudTrail/GuardDuty/Exported CW) |
| **`processed-cloudtrail-logs-ACCOUNT_ID-REGION`** | Firehose Destination cho CloudTrail logs |
| **`processed-cloudwatch-logs-ACCOUNT_ID-REGION`** | Firehose Destination cho VPC DNS/Flow logs |
| **`processed-guardduty-findings-ACCOUNT_ID-REGION`** | ETL Destination cho GuardDuty logs |
| **`athena-query-results-ACCOUNT_ID-REGION`** | Lưu trữ kết quả truy vấn Athena |

1.  Vào **S3 Console**.
2.  Đối với **mỗi bucket trong 5 buckets**:
    * Nhấn vào tên bucket.
    * Vào tab **"Objects"**.
    * Nhấn **"Empty"** để xóa tất cả dữ liệu. Bạn phải xác nhận việc xóa vĩnh viễn bằng cách gõ `permanently delete`.
    * Quay lại danh sách S3 bucket, chọn bucket, và nhấn **"Delete"**.