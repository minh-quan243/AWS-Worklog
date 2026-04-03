---
title : "Automation Setup"
date: "2000-01-01"
weight : 06
chapter : false
pre : " <b> 5.6. </b> "
---
## Phase 4: Automation Setup

-----

## Create Isolation Security Group

1.  **EC2 Console** → **Security Groups** → **Create security group**
2.  **Name**: `IR-Isolation-SG`
3.  **Description**: `Denies all inbound and outbound traffic for compromised instances`
4.  **VPC**: Select your VPC
5.  **Inbound rules**: None (deny all)
6.  **Outbound rules**: Remove default (deny all)
7.  **Create and note Security Group ID** (e.g., `sg-0078026b70389e7b3`)

## Create SNS Topic

1.  **SNS Console** → **Create topic**
2.  **Type**: Standard, **Name**: `IncidentResponseAlerts`
3.  **Access policy**:


```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "0",
      "Effect": "Allow",
      "Principal": {
        "Service": "events.amazonaws.com"
      },
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:ap-southeast-1:831981618496:IncidentResponseAlerts"
    },
    {
      "Sid": "AWSEvents_IncidentResponseAlert_Target0",
      "Effect": "Allow",
      "Principal": {
        "Service": "events.amazonaws.com"
      },
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:ap-southeast-1:831981618496:IncidentResponseAlerts"
    }
  ]
}
```
![alt text](</images/5-Workshop/Workshop pic/25.png>)
## Create Lambda Functions - Incident Response

### ir-parse-findings-lambda

  - **Handler**: `parse_findings.lambda_handler`
  - **Role**: `ParseFindingsLambdaServiceRole`
  - **Code**: [parse-findings](../5.11-Appendices/5.11-Appendices/5.11.6-parse-findings)

### ir-isolate-ec2-lambda

  - **Handler**: `isolate_ec2.lambda_handler`
  - **Role**: `IsolateEC2LambdaServiceRole`
  - **Env**: `ISOLATION_SG_ID=sg-XXXXXXX` (from step 12)
  - **Code**: [isolate-ec2](../5.11-Appendices/5.11-Appendices/5.11.7-isolate-ec2)

### ir-quarantine-iam-lambda

  - **Handler**: `quarantine_iam.lambda_handler`
  - **Role**: `QuarantineIAMLambdaServiceRole`
  - **Env**: `QUARANTINE_POLICY_ARN=arn:aws:iam::ACCOUNT_ID:policy/IrQuarantineIAMPolicy`
  - **Code**: [quarantine-iam](../5.11-Appendices/5.11-Appendices/5.11.8-quarantine-iam)

### ir-alert-dispatch

  - **Handler**: `alert_dispatch.lambda_handler`
  - **Role**: `AlertDispatchLambdaServiceRole`
  - **Env**: `SENDER_EMAIL`, `RECIPIENT_EMAIL`, `SLACK_WEBHOOK_URL`
  - **Add SNS trigger**: Topic `IncidentResponseAlerts`
  - **Code**: [alert-dispatch](../5.11-Appendices/5.11-Appendices/5.11.9-alert-dispatch)

## Update SNS Topic Subscription

1.  **SNS Console** → `IncidentResponseAlerts` → **Subscriptions**
2.  **Verify**: Protocol=AWS Lambda, Endpoint=`ir-alert-dispatch`, Status=Confirmed

## Create Step Functions State Machine

1.  **Step Functions Console** → **Create state machine**
2.  **Type**: Standard, **Name**: `IncidentResponseStepFunctions`
3.  **Definition**: [Step Functions Definition](../5.11-Appendices/5.11.10-step-functions-state-machine-definition)
4.  **Role**: `StepFunctionsRole`
5.  **Create**

## Create EventBridge Rule

1.  **EventBridge Console** → **Rules** → **Create rule**
2.  **Name**: `IncidentResponseAlert`
3.  **Event pattern**:

```json
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Finding"]
}
```

4.  **Targets (2)**:
      - **SNS topic**: `IncidentResponseAlerts`
      - **Step Functions**: `IncidentResponseStepFunctions` with role `IncidentResponseStepFunctionsEventRole`

## Configure Athena Workgroup

1.  **Athena Console** → **Workgroups** → `primary` → **Edit**
2.  **Query result location**: `s3://athena-query-results-ACCOUNT_ID-REGION/`
3.  **Save**
