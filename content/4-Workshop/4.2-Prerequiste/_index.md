---
title : "Prerequisites"
date: "2026-01-11" 
weight : 1 
chapter : false
pre : " <b> 4.2. </b> "
---

#### Requirements

Before starting the workshop, make sure you have the following in place:

- An **AWS account** with console access
- An **IAM user** with sufficient permissions (see below)
- A **GitHub repository** containing the Voice Summarizer frontend code (for the Amplify deployment step)
- Basic familiarity with the AWS Management Console

#### IAM Permissions

Your IAM user must have permissions to create and manage all services used in this workshop. Attach the following policy to your user or role before proceeding.

{{% notice warning %}}
⚠️ The policy below grants broad permissions scoped to the services used in this workshop. In a production environment, always follow the principle of least privilege and restrict permissions to specific resources.
{{% /notice %}}

![Required IAM Permissions](/images/workshop/2-prerequisites/iam-permissions.png)

The policy must cover the following services and actions:

| Service | Required Actions |
|---|---|
| **EC2** | Create/delete VPC, subnets, security groups, route tables, internet gateways, NAT gateways, endpoints, EC2 instances |
| **S3** | Create/delete buckets, put/get/delete objects, manage bucket policies and public access settings |
| **DynamoDB** | Create/delete tables, read/write items |
| **Lambda** | Create/delete functions, add/remove triggers, invoke functions |
| **IAM** | Create/delete roles and instance profiles, attach policies, pass roles |
| **Cognito** | Create/delete user pools and app clients |
| **Amplify** | Create/delete apps, branches, and deployments |
| **Transcribe** | Start and describe transcription jobs |
| **CloudWatch / Logs** | Create log groups, put retention policies |

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VoiceSummarizerWorkshop",
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "s3:*",
        "dynamodb:*",
        "lambda:*",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:AttachRolePolicy",
        "iam:DetachRolePolicy",
        "iam:PutRolePolicy",
        "iam:DeleteRolePolicy",
        "iam:GetRole",
        "iam:PassRole",
        "iam:CreateInstanceProfile",
        "iam:DeleteInstanceProfile",
        "iam:AddRoleToInstanceProfile",
        "iam:RemoveRoleFromInstanceProfile",
        "cognito-idp:*",
        "amplify:*",
        "transcribe:*",
        "logs:CreateLogGroup",
        "logs:DeleteLogGroup",
        "logs:DescribeLogGroups",
        "logs:PutRetentionPolicy",
        "cloudwatch:*"
      ],
      "Resource": "*"
    }
  ]
}
```

#### Region

{{% notice info %}}
All resources in this workshop must be created in **ap-southeast-1 (Singapore)**. Make sure the correct region is selected in the AWS Console top-right region selector before starting each section.
{{% /notice %}}
