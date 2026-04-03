---
title : "Cài đặt IAM Role và Policy cho Lambda"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.7.2.1. </b> "
---
Trong hướng dẫn này, bạn sẽ cài đặt IAM Role và Policy cho Lambda.

## Tạo IAM Role cho Lambda
1. **Mở IAM Console**
   - Điều hướng tới https://console.aws.amazon.com/iam/
   - Hoặc: AWS Management Console → Services → IAM

   ![Screenshot: IAM Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_create_role_page.png)

2. **Create Role**:
   - Chọn tùy chọn `Role` trên menu bên trái.
   - Sau đó nhấn **Create role**.

    ![Screenshot: Role page](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_create_role.png)

3. **Chọn trusted entity**:
   - Trusted entity type: **AWS Service**
   -  Use case: **Lambda**
   - Nhấn **"Next"**

    ![Screenshot: Trusted entity setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_1.png)

4. **Đính kèm permissions policies**:
   - Trong hộp tìm kiếm, nhập `AWSLambdaBasicExecutionRole`
   - Đánh dấu ô bên cạnh **"AWSLambdaBasicExecutionRole"**
   - Nhấn **"Next"**

    ![Screenshot: IAM Create role - Add permissions](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_2.png)
    
5.  **Đặt tên, xem lại, và tạo**:

      - **Role name**: Nhập `dashboard-query-role`
      - **Description**: Nhập `Execution role for Lambda function`
      - Nhấn **"Create role"**

    ![Screenshot: IAM Create role - Name and review](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_3.png)

6.  **Thêm inline policy**:

      - Sau khi tạo, bạn sẽ ở trang chi tiết role
      - Nhấn vào tab **"Permissions"**
      - Nhấn **"Add permissions"** → **"Create inline policy"**

    ![Screenshot: IAM Role permissions tab with Add permissions button](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_add_inline_policy.png)

7.  **Tạo inline policy**:

      - Nhấn vào tab **"JSON"**
      - Dán policy sau:

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

8.  **Nhấn "Next"**

9.  **Tên Policy**:

      - **Policy name**: Nhập `lambda-query-policy`
      - Nhấn **"Create policy"**

10. **Xác minh tạo role**:

      - Bạn sẽ thấy role với cả managed và inline policies được đính kèm

    ![Screenshot: IAM Create policy - Review and create](/images/5-Workshop/5.7-Dashboard-setup/5.7.2-lambda-setup/lambda_iam_role_create_result.png)