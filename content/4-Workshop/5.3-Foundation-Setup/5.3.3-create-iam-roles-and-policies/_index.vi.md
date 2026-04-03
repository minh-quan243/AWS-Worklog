---
title : "Tạo IAM Roles và Policies"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.3.3. </b> "
---

Trong phần này, bạn sẽ tạo 17 IAM roles cùng với các policies liên quan cho Lambda functions, Firehose streams, Step Functions, và các dịch vụ khác.

### Tổng quan về IAM Roles

**Lambda Execution Roles (9 roles)**:

1.  CloudTrailETLLambdaServiceRole
2.  GuardDutyETLLambdaServiceRole
3.  CloudWatchETLLambdaServiceRole
4.  CloudWatchENIETLLambdaServiceRole
5.  CloudWatchExportLambdaServiceRole
6.  ParseFindingsLambdaServiceRole
7.  IsolateEC2LambdaServiceRole
8.  QuarantineIAMLambdaServiceRole
9.  AlertDispatchLambdaServiceRole

**Service Roles (6 roles)**:
10\. CloudTrailFirehoseRole
11\. CloudWatchFirehoseRole
12\. StepFunctionsRole
13\. IncidentResponseStepFunctionsEventRole
14\. FlowLogsIAMRole
15\. GlueCloudWatchRole

**IAM Policy (1 policy)**:
16\. IrQuarantineIAMPolicy


### Nội dung
- [Tạo Lambda Execution Roles](5.3.3.1-create-lambda-excecution-roles/)
- [Tạo Service Roles](5.3.3.2-create-service-roles/)
- [Tạo IAM Policy](5.3.3.3-create-iam-policy/)
