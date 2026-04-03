---
title : "Setup S3 Bucket for Dashboard"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.7.1. </b> "
---
In this guide, you will setup a S3 to contain web files and folder.
**Important**: Replace `ACCOUNT_ID` with your AWS Account ID and `REGION` with your target region (e.g., us-east-1) in all bucket names.

### Bucket Names

**static-dashboard-bucket-ACCOUNT_ID-REGION** - Store builded web files and folder

### Bucket Creation Instructions

1. **Open the Amazon S3 Console**
   - Navigate to https://console.aws.amazon.com/s3/
   - Or: AWS Management Console → Services → S3

   ![Screenshot: AWS S3 Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/s3_page.png)

2. **Click on "Create bucket"**

   ![Screenshot: Create Bucket Button](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/S3_create.png)

3. **Bucket create setting**:
   - Keep the setting like default:
     - Bucket name: Enter `static-dashboard-bucket-ACCOUNT_ID-REGION`
       -  Example: `static-dashboard-bucket-123456789012-us-east-1`
     - Ownership: **ACLs disabled**
     - Block Public Access: **Block all public access**
     - Bucket versioning: **Disable**
     - Tags(Optional): **Add if you want**
     - Encryption: **SSE-S3**
     - Bucket key: **Enable**
     - Click **Create bucket**

4. **Verify bucket creation**:
    - You should see a success message
    - The bucket should appear in your S3 buckets list

    ![Screenshot: S3 Console showing newly created bucket](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/S3_create_result.png)

5. **Upload files and folder**:
    - Go to [Github](https://github.com/LGK2005/Dashboard-Content) to get the web content and upload to S3

    ![Screenshot: Upload web content to S3](/images/5-Workshop/5.7-Dashboard-setup/5.7.1-s3-setup/s3_upload.png)