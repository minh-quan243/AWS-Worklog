---
title : "DynamoDB Tables"
date: "2000-01-01"
weight: 3
chapter : false
pre : " <b> 4.4.3. </b> "
---

You will create **three DynamoDB tables**:

| Table Name | Partition Key | Sort Key | Purpose |
|---|---|---|---|
| `voice_status_table` | `raw_id` (String) | — | Per-recording status, progress, and stage tracked by the Celery worker |
| `memory_AI` | `raw_id` (String) | — | Chat histories and conversation memory per recording session |
| `User` | `user_id` (String) | `raw_id` (String) | User records provisioned by the Cognito Lambda trigger |

---

### Tables 1 & 2 — voice_status_table and memory_AI

These two tables share the same partition key (`raw_id`) and are created with identical steps. `voice_status_table` tracks the pipeline state of each recording (status, progress percentage, stage label, and short summary). `memory_AI` stores the rolling conversation history and compressed memory for the AI chat sessions. Both are read and written by the EC2 Celery worker and FastAPI routers.

#### Step 1 — Open DynamoDB and Click Create Table

Navigate to **DynamoDB** in the AWS Console. Click **Create table**.

![Click Create Table](/images/4-Workshop/DynamoDB/B1_Click_Create_Table.png)

---

#### Step 2 — Configure voice_status_table

Fill in the table settings for the first table:

- **Table name**: `voice_status_table`
- **Partition key**: `raw_id` — type **String**
- **Sort key**: *(leave empty)*
- **Table settings**: Default settings (On-demand capacity)

![Enter Table Name and Partition Key](/images/4-Workshop/DynamoDB/B2_Configure_VST.png)

---

#### Step 3 — Create voice_status_table

Review the settings and click **Create table**. Wait for the status to change to **Active** before proceeding.

![Click Create](/images/4-Workshop/DynamoDB/B3_Create_Table.png)

---

#### Step 4 — Repeat for memory_AI

Click **Create table** again. Use the same configuration with the new name:

- **Table name**: `memory_AI`
- **Partition key**: `raw_id` — type **String**
- **Sort key**: *(leave empty)*
- **Table settings**: Default settings (On-demand capacity)

![Configure memory_AI Table](/images/4-Workshop/DynamoDB/B4_Configure_MA.png)

Click **Create table** and wait for **Active** status.

---

### Table 3 — User

This table is written to by the Cognito post-confirmation Lambda (`lambda_user_creation_db.py`). When a user verifies their email after registration, a record is automatically created here with their `user_id`, `email`, `created_at`, and usage counters. Unlike the first two tables, the `User` table has both a partition key and a sort key.

#### Step 5 — Configure the User Table

Click **Create table** again. Configure:

- **Table name**: `User`
- **Partition key**: `user_id` — type **String**
- **Sort key**: `raw_id` — type **String**
- **Table settings**: Default settings (On-demand capacity)

![Configure User Table](/images/4-Workshop/DynamoDB/B5_Configure_User.png)

---

#### Step 6 — Verify All Three Tables Exist

In the DynamoDB console, confirm all three tables are listed with **Active** status:

![Verify All Tables](/images/4-Workshop/DynamoDB/B6_Final_Check.png)

---

{{% notice tip %}}
✅ All three DynamoDB tables are ready: <br> - **voice_status_table** — tracks recording pipeline state (status, progress, stage) <br> - **memory_AI** — stores AI chat histories and conversation memory <br> - **User** — stores user records created on Cognito sign-up <br> Set the corresponding environment variables on the EC2 instance: <br> - `STATUS_TABLE=voice_status_table` <br> - `MEMORY_TABLE=memory_AI` <br> - `USERS_TABLE` in the Lambda code maps to `User`
{{% /notice %}}