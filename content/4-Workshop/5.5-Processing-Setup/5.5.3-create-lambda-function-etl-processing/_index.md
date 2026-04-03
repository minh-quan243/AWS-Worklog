---
title : "Create Lambda Function - ETL Processing"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.5.3. </b> "
---


## Create Lambda Functions - ETL Processing

In this section, you will create 5 Lambda functions that process logs and send them to Kinesis Firehose or S3.

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

## Configure CloudWatch Logs Subscription Filter

### Configure Subscription Filter

1.  **Open the CloudWatch Console**.
2.  In the left navigation pane, select **Log Management**.

3.  Click on the centralized log group: **`/aws/incident-response/centralized-logs`**.

4.  **Create Subscription Filter**:
    * Click the **"Subscription filters"** tab.
    * Click **"Create Lambda subscription filter"**.

5.  **Configure Destination**:
    * **Destination Lambda function**: Select the function **`cloudwatch-export-lambda`**.
    * **Log format**: Select **"Other"**. (This ensures the raw log data is passed efficiently for Lambda processing).

6.  **Configure Log Format and Filter**:
    * **Subscription filter name**: Enter a descriptive name, e.g., `VPC-Log-Export-Filter`.
    * **Filter pattern**: Leave this field **blank**. (Ensures all logs in the group are processed).

7.  Click **"Start streaming"**.
   
## Configure S3 Event Notifications

S3 Console → `incident-response-log-list-bucket-ACCOUNT_ID-REGION` → Properties → Event notifications

Create 4 notifications with Event types/Object creation/✅All object create events:

1.  **CloudTrailETLTrigger**: Prefix `AWSLogs/ACCOUNT_ID/CloudTrail/` → Lambda `incident-response-cloudtrail-etl`
2.  **VPCDNSLogsTrigger**: Prefix `exportedlogs/vpc-dns-logs/` → Lambda `cloudwatch-etl-lambda`
3.  **VPCFlowLogsTrigger**: Prefix `exportedlogs/vpc-flow-logs/` → Lambda `cloudwatch-eni-etl-lambda`
4.  **GuardDutyFindingsTrigger**: Prefix `AWSLogs/ACCOUNT_ID/GuardDuty/` → Lambda `incident-response-guardduty-etl`

-----
