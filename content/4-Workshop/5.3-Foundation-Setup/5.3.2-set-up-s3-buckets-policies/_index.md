---
title : "Set up S3 buckets policies"
date: "2000-01-01"
weight : 02
chapter : false
pre : " <b> 5.3.2. </b> "
---

In this section, you will configure the bucket policy for the primary log bucket to allow CloudTrail, GuardDuty, and CloudWatch Logs to write logs.

### Configure Bucket Policy

1. **Navigate to the primary log bucket**:
   - In S3 Console, click on `incident-response-log-list-bucket-ACCOUNT_ID-REGION`

![alt text](</images/5-Workshop/Workshop pic/7 open bucket 5.3.2.png>)

2. **Open the Permissions tab**:
   - Click on the **"Permissions"** tab

3. **Scroll to Bucket policy**:
   - Scroll down to the **"Bucket policy"** section
   - Click **"Edit"**

![alt text](</images/5-Workshop/Workshop pic/9 policy edit.png>)

4. **Paste the bucket policy**:
   - Copy the following JSON policy
   - **Important**: Replace `ACCOUNT_ID` and `REGION` with your actual values in the policy

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

5.  **Click "Save changes"**

6.  **Verify policy is saved**: You should see the policy displayed in the Bucket policy section

![alt text](</images/5-Workshop/Workshop pic/10 confirm successful 5.3.2.png>)