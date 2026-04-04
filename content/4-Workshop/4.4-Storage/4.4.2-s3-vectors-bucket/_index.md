---
title : "S3 Vectors Bucket"
date: "2000-01-01"
weight: 2
chapter : false
pre : " <b> 4.4.2. </b> "
---

This bucket stores the semantic vector index for each recording, including:
- Individual chunk embeddings (for precise factual retrieval)
- Segment-level summaries (for topic-level queries)
- A global summary (for full-meeting summary queries)

The EC2 backend reads and writes to this bucket directly via the VPC Gateway Endpoint created in the networking section.

---

#### Step 1 — Click Create Bucket

Navigate to **S3** in the AWS Console. Click **Create bucket**.

![Click Create Vectors Bucket](/images/4-Workshop/S3_Vector/B1_Click_Create_Vector_Bucket.png)

---

#### Step 2 — Enter the Bucket Name and Create

- **Bucket name**: Choose a unique name, for example `voice-summarizer-vectors-<your-account-id>`
- **AWS Region**: `ap-southeast-1`
- **Block all public access**: ✅ Enabled
- **Default encryption**: SSE-S3

Note the bucket name — you will need to configure it as an environment variable on the EC2 instance.

Click **Create bucket**.

![Enter Name and Create](/images/4-Workshop/S3_Vector/B2_Create_Bucket.png)

---

{{% notice tip %}}
✅ You now have two S3 buckets configured:
- **Audio & Transcript bucket** (`one4allthing`) with `raw_audio/` and `transcripts/` folders
- **Vectors bucket** for storing the semantic index
{{% /notice %}}
