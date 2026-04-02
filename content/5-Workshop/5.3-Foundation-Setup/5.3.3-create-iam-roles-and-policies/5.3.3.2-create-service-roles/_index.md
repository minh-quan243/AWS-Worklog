---
title : "Create Service Roles"
date: "2000-01-01"
weight : 02
chapter : false
pre : " <b> 5.3.3.2. </b> "
---

### Create Firehose Roles

#### Create CloudTrailFirehoseRole

1.  **Open IAM Console** → **Roles** → **Create role**

2.  **Select trusted entity**:

      - **Trusted entity type**: **AWS service**
      - **Use case**: Select **"Kinesis"** → **"Kinesis Firehose"**
      - Click **"Next"**

![alt text](</images/5-Workshop/Workshop pic/20 5.3.3.2 Firehose Select trusted entity_.png>)

3.  **Add permissions**:

      - Skip adding managed policies (we'll add inline policy)
      - Click **"Next"**

4.  **Name and create**:

      - **Role name**: `CloudTrailFirehoseRole`
      - **Description**: `Allows Firehose to write CloudTrail logs to S3`
      - Click **"Create role"**

5.  **Add inline policy**:

      - Policy name: `FirehosePolicy`
      - Policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::processed-cloudtrail-logs-ACCOUNT_ID-REGION",
        "arn:aws:s3:::processed-cloudtrail-logs-ACCOUNT_ID-REGION/*"
      ]
    }
  ]
}
```

#### Create CloudWatchFirehoseRole

  - Role name: `CloudWatchFirehoseRole`
  - Description: `Allows Firehose to write CloudWatch logs to S3`
  - Trusted entity: Kinesis Firehose
  - Inline policy name: `FirehosePolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::processed-cloudwatch-logs-ACCOUNT_ID-REGION",
        "arn:aws:s3:::processed-cloudwatch-logs-ACCOUNT_ID-REGION/*"
      ]
    }
  ]
}
```

### Create Step Functions Role

#### Create StepFunctionsRole

1.  **Create role**:

      - **Trusted entity**: Step Functions
      - **Role name**: `StepFunctionsRole`
      - **Description**: `Execution role for Incident Response Step Functions`

2.  **Add TWO inline policies**:

**Policy 1: LambdaInvokePolicy**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "lambda:InvokeFunction",
      "Resource": [
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:ir-isolate-ec2-lambda",
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:ir-parse-findings-lambda",
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:ir-quarantine-iam-lambda"
      ]
    }
  ]
}
```

**Policy 2: EC2AutoScalingPolicy**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:DescribeAutoScalingInstances",
        "autoscaling:DetachInstances",
        "autoscaling:UpdateAutoScalingGroup",
        "ec2:CreateSnapshot",
        "ec2:CreateTags",
        "ec2:DescribeVolumes",
        "ec2:ModifyInstanceAttribute"
      ],
      "Resource": "*"
    }
  ]
}
```

### Create EventBridge Role

#### Create IncidentResponseStepFunctionsEventRole

  - Role name: `IncidentResponseStepFunctionsEventRole`
  - Description: `Allows EventBridge to trigger Step Functions`
  - Trusted entity: EventBridge
  - Inline policy name: `StartStepFunctionsPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "states:StartExecution",
      "Resource": "arn:aws:states:REGION:ACCOUNT_ID:stateMachine:IncidentResponseStepFunctions"
    }
  ]
}
```

### Create VPC Flow Logs Role

#### Create FlowLogsIAMRole

1.  **Create role**:

      - **Trusted entity**: EC2 (will edit trust policy)
      - **Role name**: `FlowLogsIAMRole`

2.  **Edit trust relationship** to:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "vpc-flow-logs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

3.  **Add inline policy**:
      - Policy name: `FlowLogsPolicy`
      - Policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

### Create Glue Role

#### Create GlueCloudWatchRole

  - Role name: `GlueCloudWatchRole`
  - Description: `Allows Glue to access S3 and CloudWatch Logs`
  - Trusted entity: Glue
  - **Managed policies** (attach 3):
      - AWSGlueServiceRole
      - CloudWatchLogsReadOnlyAccess
      - AmazonS3FullAccess
  - **No inline policies needed**