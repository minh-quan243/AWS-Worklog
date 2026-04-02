---
title: "Proposal"
date: "2025-12-05"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

![AWS Logo](/images/2-Proposal/AWSLogo.png)

# Automated AWS Incident Response and Forensics System

# Proposal Link: [Proposal](https://docs.google.com/document/d/1RcPJmiVxS80qdi0wOvHltBcSoZtRC97LngEZEkKC09k/edit?brid=hv-MPz2j8n_U1ZiCdslbyw&tab=t.0)

## 1. Executive Summary
Our team is building an automated incident response and forensics solution as part of the AWS First Cloud Journey internship program. The idea is straightforward—when a security issue happens in AWS, we want the system to respond automatically without waiting for manual intervention.

We're creating a platform that automatically detects security findings from GuardDuty, isolates affected resources, captures forensic evidence through comprehensive data collection, and provides analytics and dashboards where security teams can investigate what happened. Everything is built using Infrastructure-as-Code with AWS CDK, so customers can easily deploy it into their own AWS accounts.

## 2. Problem Statement
### What’s the Problem?
The increasing frequency and sophistication of cyber threats pose significant risks to organizations relying on cloud infrastructure. Manual incident response processes are often slow, inconsistent, and prone to human error, which can lead to prolonged system downtime, data breaches, and financial losses. The project aims to address these challenges by developing an automated, reliable, and scalable incident response system that minimizes response time, enhances forensic capabilities, and reduces operational costs.

### The Solution
The main use cases include detecting unauthorized AWS credential use, identifying compromised EC2 instances, and ensuring forensic data is properly collected, processed, and stored for investigation. Our architecture integrates VPC Flow Logs, CloudTrail, CloudWatch, and GuardDuty to detect threats, while Step Functions orchestrates the automated response workflow including EC2 isolation, ASG detachment, Create Snapshot and IAM quarantine. All evidence is collected and processed through custom ETL Lambda and Data Firehose, using Athena for forensic analysis. The system also includes alert dispatching, notification via messaging and email, and provides dashboards and analytics for security teams to investigate what happened.

### Benefits and Return on Investment
* **Rapid threat detection**: Automated response reduces the window of vulnerability.
* **Comprehensive evidence gathering**: Automated forensic data collection facilitates faster investigations.
* **Cost-effective deployment**: Leveraging AWS serverless services minimizes infrastructure expenses.
* **Improved security posture**: Continuous monitoring and real-time alerts.
* **Actionable insights**: Dashboards and analytics empower security teams.
* **Scalability**: Adaptable to organizations of various sizes and incident volumes.

## 3. Solution Architecture
Our solution uses a comprehensive multi-stage architecture for automated incident response and forensics:

![AWS Architecture](/images/2-Proposal/AWSWorkshopArchitecture-Final.png)

### AWS Services Used
- **Amazon GuardDuty**: Continuously monitors for security threats and suspicious activity.
- **AWS Step Functions**: Orchestrates the incident response workflow.
- **AWS Lambda**: Runs automation code for isolation and data processing.
- **Amazon EventBridge**: Routes findings from GuardDuty to Step Functions.
- **Amazon S3**: Stores forensic evidence and hosts static dashboard.
- **Amazon Athena**: Enables SQL queries against forensic datasets.
- **Amazon API Gateway**: Facilitates communication between dashboard and backend.
- **Amazon Cognito**: Secures access for dashboard users.
- **Amazon CloudFront**: Accelerates dashboard delivery across the globe.
- **Amazon SNS & SES**: Handles notifications via messaging and email.
- **AWS CloudTrail**: Logs all actions for auditing.
- **Amazon CloudWatch**: Monitoring and dashboards.
- **Amazon EC2**: Optional instances for analysis.
- **AWS KMS**: Key management for encryption.
- **Amazon Kinesis Data Firehose**: Streams data to S3.

### Component Design
- **Data Collection & Detection Layer**: Collects events from VPC Flow Logs, CloudTrail, CloudWatch, EC2, and GuardDuty.
- **Event Processing Layer**: Alert Dispatch, EventBridge routes findings to Step Functions; events are classified by type.
- **Automated Response Orchestration**: Step Functions handle parsing, decision making, EC2 isolation, termination protection, ASG detachment, snapshot creation, and IAM quarantine.
**Alerting & Notification Layer**: SNS, Slack & SES handles notifications via messaging and email, Alert Dispatch.
- **Data Processing & Analytics Layer**: ETL pipeline with Lambda and Data Firehose processes raw logs into S3; Athena queries the data.
- **Dashboard & Analysis Layer**: S3-hosted React dashboard with Cognito auth, consuming data via API Gateway and Athena.

## 4. Technical Implementation
### Implementation Phases
We use Agile Scrum with 1-week sprints over 6 weeks:
- **Sprint 1**: Foundation & Setup (VPC, Security Groups, Training).
- **Sprint 2**: Core Orchestration (Step Functions, Lambda, GuardDuty integration).
- **Sprint 3**: Data & Analytics (S3, Athena, ETL pipeline).
- **Sprint 4**: Dashboard & UI (Static site, API Gateway, CloudFront).
- **Sprint 5**: Testing & Optimization (Cognito, Performance testing, Simulations).
- **Sprint 6**: Documentation & Handover (Guides, Demos, Final Polish).

### Technical Requirements
- **Frontend & Dashboards**: Custom HTML/CSS/JS hosted on S3, served via CloudFront.
- **Backend & Processing**: Python 3.12 for Lambda, Step Functions for orchestration.
- **Data & Storage**: S3 for evidence, Athena for querying, Firehose for streaming.
- **Infrastructure**: All defined in AWS CDK (Python).
- **Security**: GuardDuty for detection, IAM for least privilege, KMS for encryption.

## 5. Timeline & Milestones
**Project Timeline**
**Project Timeline**
- **Week 6-7 (Foundation & Setup)**
    - **Activities**: Team training on GuardDuty/Step Functions, architecture design review, VPC and security setup.
    - **Deliverables**: Architecture document v1, team training completion, GitHub repository established.
- **Week 7-9 (Core Orchestration)**
    - **Activities**: Step Functions workflow development, Lambda function coding for all response actions, EventBridge integration, SNS/SES setup, integration testing.
    - **Deliverables**: Step Functions state machine definition, 7+ Lambda functions with documentation, GuardDuty integration, notification system, API Gateway.
- **Week 10 (Data & Analytics)**
    - **Activities**: S3 forensic storage setup, Athena table creation, ETL pipeline development, SQL query library.
    - **Deliverables**: 15+ Athena queries documented, forensic analysis runbooks, processed data storage.
- **Week 11 (Dashboard & UI)**
    - **Activities**: Static dashboard development, Cognito authentication, API Gateway setup, CloudFront CDN configuration, dashboard integration.
    - **Deliverables**: S3-hosted dashboard, authentication system, query interface, real-time results integration.
- **Week 12 (Testing & Validation & Optimization)**
    - **Activities**: Manual testing, security scanning including simulated incident scenarios (5+ workflows), performance testing, attack simulation. Optimize data with Athena query and Data Firehose.
    - **Deliverables**: Security scan results, incident simulation videos, data optimization.
- **Week 13 (Documentation & Handover)**
    - **Activities**: Deployment guide, API documentation, knowledge transfer sessions, final demo, GitHub cleanup.
    - **Deliverables**: Complete GitHub repository (public), deployment guide instructions, live workshop demonstration.

## 6. Budget Estimation
You can find the detailed budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=b9b2c0423dcd3b21dadd62e5053a5fdf2d003339).

### Infrastructure Costs
Typical monthly deployment cost (Free Tier / Low scale): **~$5.01**

- **GuardDuty**: ~$1.80/month
- **S3**: ~$1.07/month
- **KMS**: ~$1.12/month
- **CloudTrail**: ~$0.55/month
- **Athena**: ~$0.29/month
- **Amazon Simple Email Service (SES)**: ~$0.09/month
- **Amazon API Gateway**: ~$0.05/month
- **Amazon Data firehose**: ~$0.04/month
- **Lambda, Step Functions, SNS**: Generally within Free Tier limits for typical usage.


**Note**: Costs assume typical usage of 20-150 incidents per month.

## 7. Risk Assessment
### Risk Matrix
- **Performance Bottlenecks**: High data volume slowing down queries.
- **Security Breaches**: Compromise of the forensic data itself.
- **Cost Overruns**: Unchecked logging or infinite loops.

### Mitigation Strategies
- **Performance**: Monitor Athena/Firehose; optimize queries; dynamic resource adjustment.
- **Security**: Encryption (KMS), strict IAM roles, audit logging, compliance checks.
- **Cost**: AWS budget alerts, cost anomaly detection, auto-scaling limits.
- **Disaster Recovery**: Backups, failover procedures, and redundancy measures.

## 8. Expected Outcomes
### Technical Improvements
- **Automated Response**: Zero-touch isolation of compromised resources.
- **Speed**: Reduction of investigation time from hours to minutes.
- **Reliability**: Consistent, repeatable evidence collection without human error.

### Long-term Value
- **Scalable Architecture**: Foundation for future security automation.
- **Knowledge**: Team competency in advanced AWS security and serverless concepts.
- **Reusable Asset**: A deployable solution for other AWS customers or teams.

---
**Status:** Ready for Review & Approval
**Project Code:** AWS-FCJ-IR-FORENSICS-2025
