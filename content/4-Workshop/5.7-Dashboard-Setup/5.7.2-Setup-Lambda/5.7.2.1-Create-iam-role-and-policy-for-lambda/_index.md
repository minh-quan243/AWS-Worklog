---
title : "Lambda IAM Role and Policy setup"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.7.2.1. </b> "
---
In this guide, you will setup IAM Role and Policy for Lambda.

## Create IAM Role for Lambda
1. **Open the IAM Console**
   - Navigate to https://console.aws.amazon.com/iam/
   - Or: AWS Management Console → Services → IAM

   ![Screenshot: IAM Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_create_role_page.png)

2. **Create Role**:
   - Choose the `Role` option on the left menu panel.
   - Then click **Create role**.

    ![Screenshot: Role page](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_create_role.png)

3. **Select trusted entity**:
   - Trusted entity type: **AWS Service**
   -  Use case: **Lambda**
   - Click **"Next"**

    ![Screenshot: Trusted entity setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_1.png)

4. **Attach permissions policies**:
   - In the search box, type `AWSLambdaBasicExecutionRole`
   - Check the box next to **"AWSLambdaBasicExecutionRole"**
   - Click **"Next"**

    ![Screenshot: IAM Create role - Add permissions](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_2.png)
    
5.  **Name, review, and create**:

      - **Role name**: Enter `dashboard-query-role`
      - **Description**: Enter `Execution role for Lambda function`
      - Click **"Create role"**

    ![Screenshot: IAM Create role - Name and review](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_3.png)

6.  **Add inline policy**:

      - After creation, you'll be on the role details page
      - Click on the **"Permissions"** tab
      - Click **"Add permissions"** → **"Create inline policy"**

    ![Screenshot: IAM Role permissions tab with Add permissions button](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_add_inline_policy.png)

7.  **Create inline policy**:

      - Click on the **"JSON"** tab
      - Paste the following policy:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AthenaActions",
                "Effect": "Allow",
                "Action": [
                    "athena:StartQueryExecution",
                    "athena:GetQueryExecution",
                    "athena:GetQueryResults",
                    "athena:StopQueryExecution"
                ],
                "Resource": "*"
            },
            {
                "Sid": "GlueCatalogRead",
                "Effect": "Allow",
                "Action": [
                    "glue:GetDatabase",
                    "glue:GetDatabases",
                    "glue:GetTable",
                    "glue:GetTables",
                    "glue:GetPartitions"
                ],
                "Resource": "*"
            },
            {
                "Sid": "S3SourceAndResultAccess",
                "Effect": "Allow",
                "Action": [
                    "s3:GetBucketLocation",
                    "s3:GetObject",
                    "s3:ListBucket",
                    "s3:PutObject",
                    "s3:AbortMultipartUpload"
                ],
                "Resource": [
                    "arn:aws:s3:::vel-athena-results",
                    "arn:aws:s3:::vel-athena-results/*",
                    "arn:aws:s3:::vel-processed-cloudtrail-logs",
                    "arn:aws:s3:::vel-processed-cloudtrail-logs/*",
                    "arn:aws:s3:::vel-processed-guardduty",
                    "arn:aws:s3:::vel-processed-guardduty/*",
                    "arn:aws:s3:::cloudwatch-formatted",
                    "arn:aws:s3:::cloudwatch-formatted/*"
                ]
            }
        ]
    }
    ```

8.  **Click "Next"**

9.  **Policy name**:

      - **Policy name**: Enter `lambda-query-policy`
      - Click **"Create policy"**

10. **Verify role creation**:

      - You should see the role with both managed and inline policies attached

    ![Screenshot: IAM Create policy - Review and create](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_create_result.png)