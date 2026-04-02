---
title : "Prerequisites"
date: "2000-01-01" 
weight : 1 
chapter : false
pre : " <b> 5.2. </b> "
---

## Required Access and Information

Before proceeding with the setup of the **Automated AWS Incident Response and Forensics System**, ensure you have gathered the required access credentials and information below.

### 🔑 Access & Identifiers

* **AWS Account with Administrative Access**
    * You need full administrative permissions to create resources across multiple AWS services.
    * Access to the **AWS Management Console**.
* **Your AWS Account ID**
    * Format: 12-digit number (e.g., `123456789012`).
    * **Placeholder**: Replace `ACCOUNT_ID` throughout the guide.
* **Target AWS Region**
    * Choose the region where you'll deploy the system (e.g., `us-east-1`).
    * **Placeholder**: Replace `REGION` throughout the guide.
* **VPC ID**
    * A VPC with at least one subnet is required for VPC Flow Logs.
    * **Placeholder**: Replace `YOUR_VPC_ID` in the guide.
* **Amazon SES Verified Email Address**
    * Required for sending and recieving email alerts. Verify this address in the **SES Console**.
    * **Placeholder**: Replace `YOUR_VERIFIED_EMAIL@example.com`.
* **Slack Webhook URL (Optional)**
    * If you want Slack notifications, obtain a webhook URL from your Slack workspace.
    * **Placeholder**: Replace `YOUR_SLACK_WEBHOOK_URL`.