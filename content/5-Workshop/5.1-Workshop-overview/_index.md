---
title : "Overview"
date: "2000-01-01" 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### System Components
+ **Auto Incident Response and Forensics** is an architecture that uses automation services to ingest, process, and automatically respond to security findings, minimizing the time required for human intervention and aids security personel in visualizing and analyzing logs.
+ This system is built around **AWS Security Services** (CloudTrail, GuardDuty, VPC Flow Logs, CloudWatch) feeding data into a **Centralized Data Lake (S3/Glue/Athena)** for analysis.
+ The core automation is driven by **AWS EventBridge** rules triggering **AWS Step Functions** workflows, which then execute **AWS Lambda** functions to perform isolation and alerting actions.

![Workshop Architecture](/images/2-Proposal/AWSWorkshopArchitecture-Final.png)
_Workshop Architecture_

#### Workshop overview
In this workshop, you will deploy a multi-phase system to achieve end-to-end security automation. This includes:
+ **Foundation Setup**: Creating dedicated S3 buckets and IAM roles to support all services.
+ **Monitoring Setup**: Enabling and configuring key security logs (CloudTrail, GuardDuty, VPC Flow Logs) to direct data to the central log ingestion point.
+ **Processing Setup**: Deploying Kinesis Firehose, Lambda ETLs, and Glue/Athena tables to transform raw logs into an easily queryable security data lake.
+ **Automation Setup**: Creating the **Isolation Security Group**, **SNS Topic**, **Incident Response Lambda Functions**, and the **Step Functions State Machine** that executes automatic quarantine actions when GuardDuty detects findings.
+ **Dashboard Setup**: Hosting a secure, **S3-based static web interface** accelerated by **CloudFront** and protected by **Cognito** to provide analysts with real-time visualization of **forensic data** and direct query capabilities via **API Gateway**.