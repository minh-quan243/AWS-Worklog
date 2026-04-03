---
title : "Tạo Kinesis Data Firehose"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.5.1. </b> "
---

## Tạo Kinesis Data Firehose Delivery Streams

### Tạo cloudtrail-firehose-stream

1.  **Mở Kinesis Console** → **Delivery streams** → **Create delivery stream**

2.  **Cấu hình**:

      - **Source**: Direct PUT
      - **Destination**: Amazon S3
      - **Stream name**: `cloudtrail-firehose-stream`
      - **S3 bucket**: `processed-cloudtrail-logs-ACCOUNT_ID-REGION`
      - **Prefix**: `processed-cloudtrail/date=!{timestamp:yyyy-MM-dd}/`
      - **Error prefix**: `processed-cloudtrail/errors/date=!{timestamp:yyyy-MM-dd}/error-type=!{firehose:error-output-type}/`
      - **Buffer size**: 10 MB
      - **Buffer interval**: 300 seconds
      - **Compression**: GZIP
      - **IAM role**: `CloudTrailFirehoseRole`

3.  **Create delivery stream**

### Tạo vpc-dns-firehose-stream

  - **Stream name**: `vpc-dns-firehose-stream`
  - **S3 bucket**: `processed-cloudwatch-logs-ACCOUNT_ID-REGION`
  - **Prefix**: `vpc-logs/date=!{timestamp:yyyy-MM-dd}/`
  - **Error prefix**: `vpc-logs/errors/date=!{timestamp:yyyy-MM-dd}/error-type=!{firehose:error-output-type}/`
  - **IAM role**: `CloudWatchFirehoseRole`
  - *(Cài đặt buffer/compression giống như trên)*

### Tạo vpc-flow-firehose-stream
 
  - **Stream name**: `vpc-flow-firehose-stream`
  - **S3 bucket**: `processed-cloudwatch-logs-ACCOUNT_ID-REGION`
  - **Prefix**: `eni-flow-logs/date=!{timestamp:yyyy-MM-dd}/`
  - **Error prefix**: `eni-flow-logs/errors/date=!{timestamp:yyyy-MM-dd}/error-type=!{firehose:error-output-type}/`
  - **IAM role**: `CloudWatchFirehoseRole`
  - *(Cài đặt buffer/compression giống như trên)*

-----
