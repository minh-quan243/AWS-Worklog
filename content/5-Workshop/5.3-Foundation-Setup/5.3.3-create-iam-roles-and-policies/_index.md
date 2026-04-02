---
title : "Create IAM Roles and Policies"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.3.3. </b> "
---

In this section, you will create 17 IAM roles with their associated policies for Lambda functions, Firehose streams, Step Functions, and other services.

### Overview of IAM Roles

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


### Content
- [Create Lambda Execution Roles](5.3.3.1-create-lambda-excecution-roles/)
- [Create Service Roles](5.3.3.1-create-lambda-excecution-roles/)
- [Create IAM Policy](5.3.3.1-create-lambda-excecution-roles/)
