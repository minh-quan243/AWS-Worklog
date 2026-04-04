---
title: "Launching EC2 Instance"
date: 2026-04-04
weight: 6
chapter: false
pre: " <b> 4.6. </b> "
---

The EC2 instance is the core compute node of the platform. It runs three co-located processes:
- **FastAPI** — the REST API server handling upload URL generation, Q&A queries, status polling, and library access
- **Celery worker** — the async pipeline orchestrator (upload check → Transcribe wait → vectorisation)
- **Redis** — the Celery message broker

The instance is deployed in the **private subnet**, so it has no public IP. All inbound traffic arrives through the Application Load Balancer (set up in the next section), and outbound traffic to external LLM APIs exits through the NAT Gateway.

---

#### Step 1 — Open EC2 and Launch an Instance

Navigate to **EC2** in the AWS Console. Click **Launch instance**.

Configure the basics:
- **Name**: `voice-summarizer-backend`
- **AMI**: Ubuntu Server 24.04 LTS (or Amazon Linux 2023)
- **Instance type**: `t3.xlarge` *(see note below)*

![Launch EC2 Instance](/images/4-Workshop/EC2/B1_Launch_EC2.png)

{{% notice info %}}
**Instance sizing:** The `t3.xlarge` (4 vCPU, 16 GB RAM) is recommended for running the Sentence Transformer embedding model alongside FastAPI and Celery without memory pressure. A `t3.medium` (2 vCPU, 4 GB RAM) may work for light testing but is likely to OOM when loading the embedding model.
{{% /notice %}}

---

#### Step 2 — Choose the Instance Type

Select **t3.xlarge** from the instance type list.

![Choose xlarge](/images/4-Workshop/EC2/B2_Choose_Instance_Type.png)

---

#### Step 3 — Configure Network, Storage, and Launch

Scroll down to configure the remaining settings:

**Key pair:**
- Create a new key pair or select an existing one. Download and store the `.pem` file securely — you will need it to SSH into the instance for initial setup.

**Network settings:**
- **VPC**: `voice-summarizer-vpc`
- **Subnet**: `voice-summarizer-private-subnet`
- **Auto-assign public IP**: **Disable** *(the instance lives in the private subnet)*
- **Security group**: Select `voice-summarizer-ec2-sg` created in Section 3

**Storage:**
- Root volume: **30 GB gp3** (increase to 50 GB if you plan to store large model weights locally)

![Configure EC2](/images/4-Workshop/EC2/B3_EC2_Configuration.png)

Click **Launch instance**.

---

#### Initial Software Setup (via SSH through a Bastion or SSM)

{{% notice info %}}
Because the EC2 instance has no public IP, connect to it via **AWS Systems Manager Session Manager** (no SSH key required, no bastion needed) or by temporarily placing a bastion host in the public subnet. To use Session Manager, attach the `AmazonSSMManagedInstanceCore` policy to the EC2 instance role.
{{% /notice %}}

{{% notice info %}}
You can also assign a public IP to the EC2 instance and connect to it via SSH. However, this is not recommended for production environments.
{{% /notice %}}

Once connected, run the following to set up the environment:

```bash
# Update and install system dependencies
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y python3-pip python3-venv redis-server git

# Start Redis
sudo systemctl enable redis-server
sudo systemctl start redis-server

# Clone the project
git clone https://github.com/<your-org>/voice-summarizer.git
cd voice-summarizer

# Create a virtual environment and install dependencies
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Create the .env file
cat > .env << EOF
REGION=ap-southeast-1
HISTORY_TABLE=VoiceSummarizerHistory
AUDIO_BUCKET=one4allthing
VECTORS_BUCKET=<your-vectors-bucket-name>
LITELLM_MODEL=claude-sonnet-4-20250514
LITELLM_API_KEY=<your-api-key>
EOF

# Start FastAPI (background)
nohup uvicorn api.main:app --host 0.0.0.0 --port 8000 &

# Start Celery worker (background)
nohup celery -A worker.celery_app worker --loglevel=info &
```

---

#### Attach an IAM Role to the EC2 Instance

The EC2 instance needs permissions to access S3 and DynamoDB. Navigate to **EC2 → Instances**, select the instance, and click **Actions → Security → Modify IAM role**. Attach a role with the following permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:HeadObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::one4allthing",
        "arn:aws:s3:::one4allthing/*",
        "arn:aws:s3:::<your-vectors-bucket>",
        "arn:aws:s3:::<your-vectors-bucket>/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": ["dynamodb:GetItem", "dynamodb:PutItem", "dynamodb:UpdateItem", "dynamodb:Query"],
      "Resource": [
        "arn:aws:dynamodb:ap-southeast-1:*:table/VoiceSummarizerHistory",
        "arn:aws:dynamodb:ap-southeast-1:*:table/User"
      ]
    }
  ]
}
```

---

{{% notice tip %}}
✅ Your EC2 instance is running in the private subnet with FastAPI on port 8000, Celery, and Redis. In the next section you will create an Application Load Balancer to expose port 8000 to the frontend.
{{% /notice %}}
