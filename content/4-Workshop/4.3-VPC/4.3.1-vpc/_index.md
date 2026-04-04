---
title: "VPC Setup"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.3.1. </b> "
---

In this section you will create the complete network foundation for the Voice Summarizer platform: a VPC with public and private subnets, an Internet Gateway for inbound access, a NAT Gateway so the private EC2 instance can reach external LLM APIs, VPC Gateway Endpoints so the EC2 can access S3 and DynamoDB without traversing the internet, and a Security Group to protect the EC2 instance.

{{% notice info %}}
All resources in this section must be created in **ap-southeast-1 (Singapore)**.
{{% /notice %}}

---

#### Step 1 — Create the VPC

Navigate to **VPC** in the AWS Console. In the left sidebar click **Your VPCs**, then click **Create VPC**.

![Create VPC](/images/4-Workshop/VPC/B1_Create_VPC.png)

---

#### Step 2 — Configure the VPC

Fill in the VPC settings as shown:

- **Name tag**: `voice-summarizer-vpc`
- **IPv4 CIDR**: `10.0.0.0/16`
- Leave all other settings as default

Click **Create VPC**.

![Configure VPC](/images/4-Workshop/VPC/B2_Configure_VPC.png)

---

#### Step 3 — Create Subnets

In the left sidebar click **Subnets**, then click **Create subnet**. Select the VPC you just created.

You will create **two subnets** — one public (for the ALB) and one private (for EC2):

![Create Subnet](/images/4-Workshop/VPC/B3_Create_Subnet.png)

---

#### Step 4 — Configure the Subnets

Configure the subnets as follows:

**Public Subnet (for ALB):**
- **Name**: `voice-summarizer-public-subnet`
- **Availability Zone**: `ap-southeast-1a`
- **IPv4 CIDR**: `10.0.1.0/24`

**Private Subnet (for EC2):**
- **Name**: `voice-summarizer-private-subnet`
- **Availability Zone**: `ap-southeast-1a`
- **IPv4 CIDR**: `10.0.2.0/24`

Click **Create subnet**.

![Configure Subnets](/images/4-Workshop/VPC/B4_Configure_Subnet.png)

---

#### Step 5 — Navigate to Internet Gateways

In the left sidebar click **Internet Gateways**, then click **Create internet gateway**.

![Internet Gateway Menu](/images/4-Workshop/VPC/B5_Create_IGW.png)

---

#### Step 6 — Create the Internet Gateway

- **Name tag**: `voice-summarizer-igw`

Click **Create internet gateway**.

![Create IGW](/images/4-Workshop/VPC/B6_Create_IGW.png)

---

#### Step 7 — Attach the Internet Gateway to the VPC

After creation, select the new IGW and click **Actions → Attach to VPC**. Select `voice-summarizer-vpc` and confirm.

![Attach IGW to VPC](/images/4-Workshop/VPC/B7_Attach_IGW_VPC.png)

---

#### Step 8 — Create the NAT Gateway

In the left sidebar click **NAT Gateways**, then click **Create NAT gateway**.

- **Name**: `voice-summarizer-nat`
- **Subnet**: `voice-summarizer-public-subnet` *(NAT Gateway must be in the **public** subnet)*
- **Connectivity type**: Public
- Click **Allocate Elastic IP**, then click **Create NAT gateway**

{{% notice warning %}}
⚠️ The NAT Gateway is the largest cost driver in this workshop (~$43/month). Remember to delete it during cleanup when the workshop is complete.
{{% /notice %}}

![Create NAT Gateway](/images/4-Workshop/VPC/B8_Create_NAT.png)

---

#### Step 9 — Create a Private Route Table

In the left sidebar click **Route Tables**, then click **Create route table**.

- **Name**: `voice-summarizer-private-rt`
- **VPC**: `voice-summarizer-vpc`

Click **Create route table**.

![Create Private Route](/images/4-Workshop/VPC/B9_Create_Private_Route.png)

---

#### Step 10 — Confirm the Private Route Table

Verify the route table has been created and is associated with the correct VPC.

![Private Route Table Created](/images/4-Workshop/VPC/B10_Create_Private_RTB.png)

---

#### Step 11 — Edit Routes to Add the NAT Gateway

Select `voice-summarizer-private-rt`, click the **Routes** tab, then click **Edit routes**.

Add a new route:
- **Destination**: `0.0.0.0/0`
- **Target**: Select **NAT Gateway** → `voice-summarizer-nat`

Click **Save changes**. Then go to **Subnet associations** and associate this route table with `voice-summarizer-private-subnet`.

![Edit Routes](/images/4-Workshop/VPC/B11_Edit_RTB.png)

---

#### Step 12 — Create VPC Gateway Endpoints for S3 and DynamoDB

Gateway Endpoints allow the EC2 instance in the private subnet to access S3 and DynamoDB without going through the NAT Gateway, improving both security and cost.

In the left sidebar click **Endpoints**, then click **Create endpoint**.

Create **two endpoints** (repeat for each):

**Endpoint 1 — S3:**
- **Name**: `voice-summarizer-s3-endpoint`
- **Service category**: AWS services
- **Service**: Search for `com.amazonaws.ap-southeast-1.s3` → select the **Gateway** type
- **VPC**: `voice-summarizer-vpc`
- **Route tables**: Select `voice-summarizer-private-rt`

**Endpoint 2 — DynamoDB:**
- **Name**: `voice-summarizer-dynamodb-endpoint`
- **Service**: `com.amazonaws.ap-southeast-1.dynamodb` → **Gateway** type
- **VPC**: `voice-summarizer-vpc`
- **Route tables**: Select `voice-summarizer-private-rt`

![Create Gateway Endpoints](/images/4-Workshop/VPC/B12_Create_Endpoint_DynamoDB_S3.png)

---

#### Step 13 — Create a Security Group for EC2

In the left sidebar click **Security Groups**, then click **Create security group**.

- **Name**: `voice-summarizer-ec2-sg`
- **Description**: Security group for Voice Summarizer EC2
- **VPC**: `voice-summarizer-vpc`

**Inbound rules:**

| Type | Protocol | Port | Source | Description |
|---|---|---|---|---|
| Custom TCP | TCP | 8000 | ALB Security Group (or `10.0.0.0/16`) | FastAPI |
| SSH | TCP | 22 | Your IP | Admin access |

**Outbound rules:** Allow all traffic (default).

Click **Create security group**.

![Create Security Group](/images/4-Workshop/VPC/B13_Create_SG.png)

---

#### Step 14 — Add a Route for External LLM API Access (Bedrock/NAT)

If you are routing LLM calls to AWS Bedrock, add a route in the private route table to ensure traffic is correctly directed.

Navigate back to **Route Tables → voice-summarizer-private-rt → Routes → Edit routes** and confirm the NAT Gateway route `0.0.0.0/0` is present and active.

![Bedrock NAT Route](/images/4-Workshop/VPC/B14_Add_Route_LLM_API.png)

---

#### Step 15 — Verify Bedrock / External LLM Connectivity Configuration

Confirm the routing configuration is complete: the private subnet route table has `0.0.0.0/0 → NAT Gateway` for internet-bound LLM API calls, and the Gateway Endpoints handle S3 and DynamoDB traffic privately.

![Verify Bedrock Config](/images/4-Workshop/VPC/B15_Verify_Connection.png)

---

#### Step 16 — Assign the Public Subnet to the ALB

Navigate to **Subnets**, select `voice-summarizer-public-subnet`, and click **Actions → Edit subnet settings**. Enable **Auto-assign public IPv4 address**. This ensures the ALB (deployed in the next section) can receive public traffic.

![ALB Subnet Setting](/images/4-Workshop/VPC/B16_Assign_PubSubnet_ALB.png)

---

{{% notice tip %}}
✅ You have completed the networking setup. Your VPC now has:

- A **public subnet** with internet access via the Internet Gateway (for the ALB)
- A **private subnet** with outbound-only access via the NAT Gateway (for EC2)
- **Gateway Endpoints** for private S3 and DynamoDB access
- A **security group** protecting the EC2 instance
{{% /notice %}}
