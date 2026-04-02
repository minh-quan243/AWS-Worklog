---
title : "Create Lambda Execution Roles"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.3.3.1. </b> "
---


#### Create CloudTrailETLLambdaServiceRole

1.  **Open the IAM Console**:

      - Navigate to https://console.aws.amazon.com/iam/
      - Or: AWS Management Console → Search for "IAM" → Click "IAM"

![alt text](</images/5-Workshop/Workshop pic/11 iam console 5.3.3.1.png>)

2.  **Navigate to Roles**:

      - In the left sidebar, click **"Roles"**

3.  **Click "Create role"**

![alt text](</images/5-Workshop/Workshop pic/12 create role.png>)

4.  **Select trusted entity**:

      - **Trusted entity type**: Select **"AWS service"**
      - **Use case**: Select **"Lambda"**
      - Click **"Next"**

![alt text](</images/5-Workshop/Workshop pic/13 create role setting.png>)

5.  **Add permissions**:

      - In the search box, type `AWSLambdaBasicExecutionRole`
      - Check the box next to **"AWSLambdaBasicExecutionRole"**
      - Click **"Next"**

![alt text](</images/5-Workshop/Workshop pic/14 add permission.png>)

6.  **Name, review, and create**:

      - **Role name**: Enter `CloudTrailETLLambdaServiceRole`
      - **Description**: Enter `Execution role for CloudTrail ETL Lambda function`
      - Click **"Create role"**

![alt text](</images/5-Workshop/Workshop pic/15 Name, review, and create.png>)

7.  **Add inline policy**:

      - After creation, you'll be on the role details page
      - Click on the **"Permissions"** tab
      - Click **"Add permissions"** → **"Create inline policy"**

![alt text](</images/5-Workshop/Workshop pic/16 Add inline policy.png>)

8.  **Create inline policy**:

      - Click on the **"JSON"** tab
      - Paste the following policy (replace `ACCOUNT_ID` and `REGION`):

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "firehose:PutRecord",
        "firehose:PutRecordBatch"
      ],
      "Resource": "arn:aws:firehose:REGION:ACCOUNT_ID:deliverystream/cloudtrail-firehose-stream"
    }
  ]
}
```

![alt text](</images/5-Workshop/Workshop pic/17 Create inline policy .png>)

9.  **Click "Next"**

10. **Policy name**:

      - **Policy name**: Enter `CloudTrailETLPolicy`
      - Click **"Create policy"**

11. **Verify role creation**:

      - You should see the role with both managed and inline policies attached

![alt text](</images/5-Workshop/Workshop pic/19 Verify role creation_.png>)
#### Create Remaining Lambda Roles

**Follow the same process for each role below** (steps 3-11):

**GuardDutyETLLambdaServiceRole**

  - Role name: `GuardDutyETLLambdaServiceRole`
  - Description: `Execution role for GuardDuty ETL Lambda function`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `GuardDutyETLPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*",
        "arn:aws:s3:::processed-guardduty-findings-ACCOUNT_ID-REGION/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "kms:Decrypt",
      "Resource": "arn:aws:kms:REGION:ACCOUNT_ID:key/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreatePartition",
        "glue:GetPartition"
      ],
      "Resource": [
        "arn:aws:glue:REGION:ACCOUNT_ID:catalog",
        "arn:aws:glue:REGION:ACCOUNT_ID:database/security_logs",
        "arn:aws:glue:REGION:ACCOUNT_ID:table/security_logs/processed_guardduty"
      ]
    }
  ]
}
```

**CloudWatchETLLambdaServiceRole**

  - Role name: `CloudWatchETLLambdaServiceRole`
  - Description: `Execution role for VPC DNS logs ETL Lambda`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `CloudWatchETLPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "firehose:PutRecord",
        "firehose:PutRecordBatch"
      ],
      "Resource": "arn:aws:firehose:REGION:ACCOUNT_ID:deliverystream/vpc-dns-firehose-stream"
    }
  ]
}
```

**CloudWatchENIETLLambdaServiceRole**

  - Role name: `CloudWatchENIETLLambdaServiceRole`
  - Description: `Execution role for VPC Flow logs ETL Lambda`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `CloudWatchENIETLPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "firehose:PutRecord",
        "firehose:PutRecordBatch"
      ],
      "Resource": "arn:aws:firehose:REGION:ACCOUNT_ID:deliverystream/vpc-flow-firehose-stream"
    }
  ]
}
```

**CloudWatchExportLambdaServiceRole**

  - Role name: `CloudWatchExportLambdaServiceRole`
  - Description: `Execution role for CloudWatch log export Lambda`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `CloudWatchExportPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateExportTask",
        "logs:DescribeExportTasks",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:logs:REGION:ACCOUNT_ID:log-group:*",
        "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
      ]
    }
  ]
}
```

**ParseFindingsLambdaServiceRole**

  - Role name: `ParseFindingsLambdaServiceRole`
  - Description: `Execution role for parsing GuardDuty findings`
  - Managed policy: AWSLambdaBasicExecutionRole
  - **No inline policy needed**

**IsolateEC2LambdaServiceRole**

  - Role name: `IsolateEC2LambdaServiceRole`
  - Description: `Execution role for isolating compromised EC2 instances`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `IsolateEC2Policy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:ModifyInstanceAttribute"
      ],
      "Resource": "*"
    }
  ]
}
```

**QuarantineIAMLambdaServiceRole**

  - Role name: `QuarantineIAMLambdaServiceRole`
  - Description: `Execution role for quarantining compromised IAM users`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `QuarantineIAMPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:AttachUserPolicy",
        "iam:ListAttachedUserPolicies"
      ],
      "Resource": [
        "arn:aws:iam::ACCOUNT_ID:user/*",
        "arn:aws:iam::ACCOUNT_ID:policy/IrQuarantineIAMPolicy"
      ]
    }
  ]
}
```

**AlertDispatchLambdaServiceRole**

  - Role name: `AlertDispatchLambdaServiceRole`
  - Description: `Execution role for dispatching alerts via SNS/SES/Slack`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `AlertDispatchPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:REGION:ACCOUNT_ID:IncidentResponseAlerts"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ses:SendEmail",
        "ses:SendRawEmail"
      ],
      "Resource": "*"
    }
  ]
}
```