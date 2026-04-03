---
title : "Thiết lập S3 buckets policies"
date: "2000-01-01"
weight : 02
chapter : false
pre : " <b> 5.3.2. </b> "
---

Trong phần này, bạn sẽ cấu hình bucket policy cho bucket log chính để cho phép CloudTrail, GuardDuty, và CloudWatch Logs ghi log.

### Cấu hình Bucket Policy

1. **Điều hướng đến bucket log chính**:
   - Trong S3 Console, nhấn vào `incident-response-log-list-bucket-ACCOUNT_ID-REGION`

![alt text](</images/5-Workshop/Workshop pic/7 open bucket 5.3.2.png>)

2. **Mở tab Permissions**:
   - Nhấn vào tab **"Permissions"**

3. **Cuộn đến Bucket policy**:
   - Cuộn xuống phần **"Bucket policy"**
   - Nhấn **"Edit"**

![alt text](</images/5-Workshop/Workshop pic/9 policy edit.png>)

4. **Dán bucket policy**:
   - Copy JSON policy sau
   - **Quan trọng**: Thay thế `ACCOUNT_ID` và `REGION` bằng giá trị thực tế của bạn trong policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowGuardDutyPutObject",
      "Effect": "Allow",
      "Principal": {
        "Service": "guardduty.amazonaws.com"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "ACCOUNT_ID"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:guardduty:REGION:ACCOUNT_ID:detector/*"
        }
      }
    },
    {
      "Sid": "AllowGuardDutyGetBucketLocation",
      "Effect": "Allow",
      "Principal": {
        "Service": "guardduty.amazonaws.com"
      },
      "Action": "s3:GetBucketLocation",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "ACCOUNT_ID"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:guardduty:REGION:ACCOUNT_ID:detector/*"
        }
      }
    },
    {
      "Sid": "AllowCloudWatchLogsGetBucketAcl",
      "Effect": "Allow",
      "Principal": {
        "Service": "logs.REGION.amazonaws.com"
      },
      "Action": "s3:GetBucketAcl",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "ACCOUNT_ID"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:logs:REGION:ACCOUNT_ID:log-group:*"
        }
      }
    },
    {
      "Sid": "AllowCloudWatchLogsPutObject",
      "Effect": "Allow",
      "Principal": {
        "Service": "logs.REGION.amazonaws.com"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "ACCOUNT_ID"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:logs:REGION:ACCOUNT_ID:log-group:*"
        }
      }
    },
    {
      "Sid": "AllowCloudTrailAclCheck",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "s3:GetBucketAcl",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "ACCOUNT_ID"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:cloudtrail:REGION:ACCOUNT_ID:trail/*"
        }
      }
    },
    {
      "Sid": "AllowCloudTrailWrite",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/AWSLogs/ACCOUNT_ID/*",
      "Condition": {
        "StringEquals": {
          "s3:x-amz-acl": "bucket-owner-full-control",
          "aws:SourceAccount": "ACCOUNT_ID"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:cloudtrail:REGION:ACCOUNT_ID:trail/*"
        }
      }
    }
  ]
}
````

5.  **Nhấn "Save changes"**

6.  **Xác minh policy đã lưu**: Bạn sẽ thấy policy hiển thị trong phần Bucket policy

![alt text](</images/5-Workshop/Workshop pic/10 confirm successful 5.3.2.png>)
