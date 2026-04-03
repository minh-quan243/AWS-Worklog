---
title: "Week 10 Worklog"
date: 2026-03-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Deploy a **complete real-world project on AWS**.
* Apply learned services to build an end-to-end system.
* Get familiar with the full process:
  * System architecture design
  * Deployment
  * Testing
  * Evaluation and optimization
* Start building a system based on **serverless + event-driven architecture**.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Analyze project requirements <br>&emsp; + Define system input/output <br>&emsp; + Design data processing flow <br> - Propose serverless architecture <br>&emsp; + S3 → Lambda → SQS → Transcribe → DynamoDB <br> - Identify required AWS services | 03/16/2026 | 03/16/2026 | |
| Tue | - Set up **IAM** <br>&emsp; + Create roles for Lambda and services <br>&emsp; + Apply principle of least privilege <br> - Create **S3 bucket** <br>&emsp; + Configure audio file uploads <br> - Initialize **DynamoDB** <br>&emsp; + Design table for metadata and results | 03/17/2026 | 03/17/2026 | |
| Wed | - Build **Lambda functions** <br>&emsp; + Handle events from S3 <br>&emsp; + Send messages to SQS <br> - Create **API Gateway** <br>&emsp; + Expose endpoint for querying results <br> - Configure triggers <br>&emsp; + S3 → Lambda <br>&emsp; + Lambda → SQS | 03/18/2026 | 03/18/2026 | |
| Thu | - Integrate **AWS Transcribe** <br>&emsp; + Convert audio to text <br> - Combine with **SQS** <br>&emsp; + Handle asynchronous message processing <br>&emsp; + Avoid system blocking <br> - Store results in DynamoDB | 03/19/2026 | 03/19/2026 | |
| Fri | - Test the system <br>&emsp; + Upload audio files → verify pipeline <br>&emsp; + Validate API responses <br> - Debug and optimize <br>&emsp; + Check CloudWatch logs <br>&emsp; + Adjust timeout and permissions <br> - Evaluate performance and finalize system | 03/20/2026 | 03/20/2026 | |

---

### Week 10 Achievements:

* Designed system architecture:
  * S3 → triggers Lambda
  * Lambda → sends messages via SQS
  * SQS → handles asynchronous processing
  * Transcribe → processes audio
  * DynamoDB → stores results
  * API Gateway → provides endpoints

* Applied security best practices:
  * Used IAM roles appropriately
  * Avoided hardcoding credentials
  * Controlled access between services

* Built a serverless backend:
  * Implemented Lambda functions for business logic
  * Created APIs for querying data

* Understood system data flow:
  * Event-driven processing
  * Asynchronous workflows
  * Decoupling between components

* Performed system testing:
  * Tested with real data (audio files)
  * Debugged issues using CloudWatch logs
  * Optimized configurations for stable performance