---
title: "Clean Up"
date: 2026-04-04
weight: 10
chapter: false
pre: " <b> 4.10. </b> "
---

Congratulations on completing the Voice Summarizer workshop! 🎉

In this workshop you built a complete AI-powered audio intelligence platform on AWS, including:
- A **VPC** with public/private subnets, Internet Gateway, NAT Gateway, and Gateway Endpoints
- **Three S3 buckets** for audio, transcripts, and vector embeddings
- **Two DynamoDB tables** for recording status and user data
- **Two Lambda functions** for transcription triggering and user provisioning
- An **EC2 instance** running FastAPI, Celery, and Redis
- An **Application Load Balancer** routing traffic to the private subnet
- A **Cognito User Pool** with Post Confirmation trigger
- An **Amplify** deployment serving the React frontend from GitHub

To avoid ongoing charges, delete all resources in the order below. **Always delete resources before the networking components they depend on.**

{{% notice warning %}}
⚠️ The **NAT Gateway** (~$43/month) and **ALB** (~$18/month) and **EC2** (~$15/month) are the largest cost drivers. If you are pausing rather than finishing the workshop, at minimum **stop the EC2 instance**, **delete the NAT Gateway**, and **delete the ALB** to halt the majority of charges.
{{% /notice %}}

---

### Step 1 — Delete the Amplify App

Navigate to **AWS Amplify**. Select `voice-summarizer` and click **Actions → Delete app**. Confirm deletion.

This removes the CDN distribution and all build artifacts.

---

### Step 2 — Delete the Cognito User Pool

Navigate to **Amazon Cognito → User pools**. Select `voice-summarizer-user-pool`. Click **Actions → Delete**. Type the pool name to confirm and click **Delete**.

{{% notice info %}}
Deleting the User Pool permanently removes all user accounts. There is no undo.
{{% /notice %}}

---

### Step 3 — Delete Lambda Functions

Navigate to **Lambda → Functions**. Select each function and click **Actions → Delete**:

- `audio2text`
- `user_creation_db`

Confirm deletion for each.

---

### Step 4 — Delete the Application Load Balancer

Navigate to **EC2 → Load Balancing → Load Balancers**. Select `voice-summarizer-alb`, click **Actions → Delete load balancer**. Confirm.

Then navigate to **Target Groups**, select `voice-summarizer-tg`, and click **Actions → Delete**.

---

### Step 5 — Terminate the EC2 Instance

Navigate to **EC2 → Instances**. Select `voice-summarizer-backend`. Click **Instance state → Terminate instance**. Confirm termination.

Wait for the instance state to change to **Terminated** before proceeding with VPC cleanup.

---

### Step 6 — Empty and Delete the S3 Buckets

S3 buckets must be emptied before they can be deleted.

Navigate to **S3**. For each of the three buckets (`one4allthing`, and your vectors bucket):

1. Click the bucket name
2. Click **Empty** — type `permanently delete` and confirm
3. After emptying, click **Delete** — type the bucket name and confirm

Repeat for all three buckets.

---

### Step 7 — Delete the DynamoDB Tables

Navigate to **DynamoDB → Tables**. Select each table and click **Delete**:

- `VoiceSummarizerHistory`
- `User`

Confirm deletion for each. Note that DynamoDB on-demand tables delete quickly.

---

### Step 8 — Delete the NAT Gateway

Navigate to **VPC → NAT Gateways**. Select `voice-summarizer-nat`. Click **Actions → Delete NAT gateway**. Confirm.

{{% notice info %}}
NAT Gateways take a few minutes to fully delete. Wait until the status shows **Deleted** before releasing the associated Elastic IP.
{{% /notice %}}

After deletion, navigate to **VPC → Elastic IPs**. Select the Elastic IP that was allocated for the NAT Gateway (status will show **Available**). Click **Actions → Release Elastic IP address**. Confirm.

---

### Step 9 — Delete the VPC Endpoints

Navigate to **VPC → Endpoints**. Select both endpoints:

- `voice-summarizer-s3-endpoint`
- `voice-summarizer-dynamodb-endpoint`

Click **Actions → Delete VPC endpoints**. Confirm.

---

### Step 10 — Delete the Internet Gateway

Navigate to **VPC → Internet Gateways**. Select `voice-summarizer-igw`.

First, **detach it from the VPC**: Click **Actions → Detach from VPC**, confirm.  
Then, **delete it**: Click **Actions → Delete internet gateway**, confirm.

---

### Step 11 — Delete Route Tables

Navigate to **VPC → Route Tables**. Select `voice-summarizer-private-rt`. Click **Actions → Delete route table**. Confirm.

The main route table associated with the VPC will be deleted automatically when the VPC is deleted.

---

### Step 12 — Delete Subnets

Navigate to **VPC → Subnets**. Select each subnet and click **Actions → Delete subnet**:

- `voice-summarizer-public-subnet`
- `voice-summarizer-private-subnet`

---

### Step 13 — Delete the Security Group

Navigate to **VPC → Security Groups**. Select `voice-summarizer-ec2-sg`. Click **Actions → Delete security groups**. Confirm.

---

### Step 14 — Delete the VPC

Navigate to **VPC → Your VPCs**. Select `voice-summarizer-vpc`. Click **Actions → Delete VPC**. Confirm.

---

### Step 15 — Delete IAM Roles

Navigate to **IAM → Roles**. Search for and delete the execution roles created for:

- The `audio2text` Lambda
- The `user_creation_db` Lambda
- The EC2 instance profile

---

### Verify No Remaining Charges

After completing all steps above, verify in the AWS Console:

| Service | Expected Status |
|---|---|
| EC2 Instances | No running instances |
| NAT Gateways | No gateways (state: Deleted) |
| Load Balancers | No active load balancers |
| Elastic IPs | No allocated IPs |
| S3 Buckets | Workshop buckets deleted |
| DynamoDB Tables | Workshop tables deleted |
| Lambda Functions | Workshop functions deleted |
| Cognito User Pools | Workshop pool deleted |
| Amplify Apps | Workshop app deleted |

You can also check **AWS Cost Explorer** or **AWS Budgets** for any residual charges from services that bill in arrears.

---

{{% notice tip %}}
✅ All workshop resources have been cleaned up. Thank you for completing the Voice Summarizer AWS Workshop!
{{% /notice %}}
