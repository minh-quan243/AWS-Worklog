---
title : "Verify Setup"
date: "2000-01-01"
weight : 08
chapter : false
pre : " <b> 5.8. </b> "
---

After all the setup phase, please refer to the checklist to ensure complete resources creation

## Verify Setup

**Complete Verification Checklist:**
- **Incident Response and Forensics:**
  - ✅ **S3 Buckets**: All 5 created with versioning/encryption
  - ✅ **IAM Roles**: All 17 roles with correct policies
  - ✅ **CloudTrail**: Logging enabled
  - ✅ **GuardDuty**: Enabled with S3 export
  - ✅ **VPC Flow Logs**: Active
  - ✅ **Lambda Functions**: All 9 deployed
  - ✅ **Firehose Streams**: All 3 active
  - ✅ **Glue Tables**: All 4 created
  - ✅ **S3 Events**: All 4 triggers configured
  - ✅ **SNS Topic**: Created with subscription
  - ✅ **Step Functions**: Active
  - ✅ **EventBridge Rule**: Enabled with 2 targets

- **Security Dashboard:**
  - ✅ **S3 Buckets**: Bucket is created with dashboard file stored and enabled hosting
  - ✅ **Query Lambda**: Lambda is created with the appropriate roles
  - ✅ **API Gateway**: API Gateway is created with the correct API and resources
  - ✅ **CloudFront**: Distribution is created with API and S3 origins configured
  - ✅ **Cognito**: Linked to CloudFront distribution and created user in user pool


**End-to-End Test**

1.  **Generate sample GuardDuty findings**: 
   1.1 GuardDuty Console → Settings → Generate sample findings (200+ findings)
   or
   1.2 Trigger single finding via CloudShell (Dectector Id is in GuardDuty Console → Settings )
```bash
   aws guardduty create-sample-findings --detector-id [$dectector-id] --finding-types "Recon:EC2/PortProbeUnprotectedPort"

```
1.  **Monitor workflow**: Check EventBridge, SNS, Step Functions, Lambda logs
2.  **Verify alerts**: Check email and Slack
3.  **Query data in Athena**:
