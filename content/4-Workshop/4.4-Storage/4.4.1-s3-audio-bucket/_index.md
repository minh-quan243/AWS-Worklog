---
title : "S3 Audio & Transcript Bucket"
date: "2000-01-01"
weight: 1
chapter : false
pre : " <b> 4.4.1. </b> "
---

This bucket stores two types of data under separate prefixes:
- `raw_audio/` — original audio files uploaded by users
- `transcripts/` — JSON transcript output from AWS Transcribe

The Lambda function uses the bucket name `one4allthing` hardcoded in its configuration. Use this name (or update the Lambda code to match your chosen name).

---

#### Step 1 — Open the S3 Console and Create a Bucket

Navigate to **S3** in the AWS Console. Click **Create bucket**.

![Click Create Bucket](/images/4-Workshop/S3_Bucket/B1_Click_Create_Bucket.png)

---

#### Step 2 — Enter the Bucket Name

- **Bucket name**: `one4allthing`
- **AWS Region**: `ap-southeast-1`

{{% notice info %}}
The bucket name must be globally unique. If `one4allthing` is already taken, choose a unique name and update the `OUTPUT_BUCKET` variable in the `audio2text` Lambda function code accordingly.
{{% /notice %}}

![Enter Bucket Name](/images/4-Workshop/S3_Bucket/B2_Naming.png)

---

#### Step 3 — Block All Public Access

Under **Block Public Access settings for this bucket**, ensure all four checkboxes are **ticked** (all public access blocked). This bucket should never be publicly accessible.

![Block Public Access](/images/4-Workshop/S3_Bucket/B3_Block_All_Public_Access.png)

---

#### Step 4 — Enable Server-Side Encryption and Create

Under **Default encryption**:
- **Encryption type**: SSE-S3 (Amazon S3 managed keys)

Leave all other settings as default and click **Create bucket**.

![Enable SSE-S3 and Create](/images/4-Workshop/S3_Bucket/B4_Enable_SS_Encryption.png)

---

#### Step 5 — Open the Bucket and Create Folders

Click on the newly created bucket to open it. Click **Create folder** to add the first prefix.

![Open Bucket and Create Folder](/images/4-Workshop/S3_Bucket/B5_Create_Folder_Prefix.png)

---

#### Step 6 — Create the `raw_audio/` Folder

- **Folder name**: `raw_audio`

Click **Create folder**. Repeat steps 5–6 to also create a `transcripts/` folder.

| Folder | Purpose |
|---|---|
| `raw_audio/` | Receives audio files uploaded via presigned URLs |
| `transcripts/` | Receives JSON output from AWS Transcribe |

![Create raw_audio Folder](/images/4-Workshop/S3_Bucket/B6_Create_Presigned_Prefix.png)

---

{{% notice tip %}}
✅ Your S3 Audio bucket is ready with two folders: `raw_audio/` and `transcripts/`. The Lambda function will use `raw_audio/` as the trigger prefix and write results to `transcripts/`.
{{% /notice %}}