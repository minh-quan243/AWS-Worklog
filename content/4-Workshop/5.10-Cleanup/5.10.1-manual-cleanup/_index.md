---
title : "Manual Cleanup"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.10.1 </b> "
---

## Clean up (Manual Infrastructure Setup)

### Phase 1: Automation and Monitoring Cleanup 

The goal here is to **stop all active processes** and delete the monitoring and core automation resources (EventBridge, Step Functions, SNS, GuardDuty, Flow Logs, CloudTrail).

#### 1. Delete Incident Response Automation

1.1 **Delete EventBridge Rule**
* Go to **EventBridge Console** → **Rules**.
* Select the rule: **`IncidentResponseAlert`**.
* Click **"Delete"**.

1.2 **Delete Step Functions State Machine**
* Go to **Step Functions Console** → **State Machines**.
* Select the State Machine: **`IncidentResponseStepFunctions`**.
* Click **"Delete"**.

1.3 **Delete SNS Topic and Subscription**
* Go to **SNS Console** → **Topics** → **`IncidentResponseAlerts`**.
* First, delete the subscription associated with **`ir-alert-dispatch`**.
* Then, delete the topic itself by clicking **"Delete topic"**.

1.4 **Delete GuardDuty Detector**
* Go to **GuardDuty Console** → **Settings** → **General**.
* Click **"Suspend"** to stop processing, then click **"Disable GuardDuty"** (or **"Delete detector"**).

1.5 **Disable VPC Flow Logs**
* Go to **VPC Console** → **VPC Flow Logs**.
* Select the flow log created (associated with `YOUR_VPC_ID`).
* Click **"Delete flow log"**.

1.6 **Delete CloudTrail Trail**
* Go to **CloudTrail Console** → **Trails**.
* Select the trail: **`incident-responses-cloudtrail-ACCOUNT_ID-REGION`**.
* Click **"Delete"**.

---

### Phase 2: Lambda and Compute Cleanup 

#### 2. Delete All Lambda Functions (9 Functions)

Go to the **Lambda Console** and delete the following functions:

* `incident-response-cloudtrail-etl`
* `incident-response-guardduty-etl`
* `cloudwatch-etl-lambda`
* `cloudwatch-eni-etl-lambda`
* `cloudwatch-export-lambda`
* `ir-parse-findings-lambda`
* `ir-isolate-ec2-lambda`
* `ir-quarantine-iam-lambda`
* `ir-alert-dispatch`

#### 3. Delete Isolation Security Group 

* Go to **EC2 Console** → **Security Groups**.
* Find and select the Security Group: **`IR-Isolation-SG`** (using ID `sg-XXXXXXX`).
* Click **"Delete security group"**.

#### 4. Delete CloudWatch Log Groups 

Go to the **CloudWatch Console** → **Log Groups** and delete:

* The centralized log group: **`/aws/incident-response/centralized-logs`**.
* Any associated **Lambda log groups** for the 9 deleted functions (e.g., `/aws/lambda/ir-parse-findings-lambda`).

---

### Phase 3: Processing and Data Lake Cleanup 

#### 5. Delete Kinesis Data Firehose Streams

Go to the **Kinesis Console** → **Delivery Streams** and delete:

* `cloudtrail-firehose-stream`
* `vpc-dns-firehose-stream`
* `vpc-flow-firehose-stream`

#### 6. Delete AWS Glue Tables and Database 

6.1 **Delete Glue Tables**
* Go to **Glue Console** → **Tables**.
* Select and delete: **`security_logs.processed_cloudtrail`**, **`security_logs.processed_guardduty`**, **`security_logs.vpc_logs`**, and **`security_logs.eni_flow_logs`**.

6.2 **Delete Glue Database**
* Go to **Glue Console** → **Databases**.
* Select the database: **`security_logs`** and click **"Delete"**.

#### 7. Delete IAM Roles and Policies 

7.1 **Delete IAM Policies**
* Go to **IAM Console** → **Policies**.
* Delete the custom managed policy: **`IrQuarantineIAMPolicy`**.
* *Note: Inline policies created in the setup will be deleted automatically when the corresponding role is deleted.*

7.2 **Delete IAM Roles**
* Go to **IAM Console** → **Roles**.
* Delete the following 17 roles:
    * **Lambda Execution Roles**: `CloudTrailETLLambdaServiceRole`, `GuardDutyETLLambdaServiceRole`, `CloudWatchETLLambdaServiceRole`, `CloudWatchENIETLLambdaServiceRole`, `CloudWatchExportLambdaServiceRole`, `ParseFindingsLambdaServiceRole`, `IsolateEC2LambdaServiceRole`, `QuarantineIAMLambdaServiceRole`, `AlertDispatchLambdaServiceRole`.
    * **Service Roles**: `CloudTrailFirehoseRole`, `CloudWatchFirehoseRole`, `StepFunctionsRole`, `IncidentResponseStepFunctionsEventRole`, `FlowLogsIAMRole`, `GlueCloudWatchRole`.

---

### Phase 4: S3 Bucket Cleanup (Data Deletion) 

#### 8. Empty and Delete S3 Buckets

This is the **final step** to ensure all storage charges are stopped.

| Bucket Name | Purpose |
| :--- | :--- |
| **`incident-response-log-list-bucket-ACCOUNT_ID-REGION`** | Primary Log Source (CloudTrail/GuardDuty/Exported CW) |
| **`processed-cloudtrail-logs-ACCOUNT_ID-REGION`** | Firehose Destination for CloudTrail logs |
| **`processed-cloudwatch-logs-ACCOUNT_ID-REGION`** | Firehose Destination for VPC DNS/Flow logs |
| **`processed-guardduty-findings-ACCOUNT_ID-REGION`** | ETL Destination for GuardDuty logs |
| **`athena-query-results-ACCOUNT_ID-REGION`** | Athena Query Results Storage |

1.  Go to the **S3 Console**.
2.  For **each of the 5 buckets**:
    * Click on the bucket name.
    * Go to the **"Objects"** tab.
    * Click **"Empty"** to clear all data. You must confirm the permanent delete by typing `permanently delete`.
    * Go back to the S3 bucket list, select the bucket, and click **"Delete"**.