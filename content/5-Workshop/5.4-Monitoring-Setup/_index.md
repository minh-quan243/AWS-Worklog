---
title : "Monitoring Setup"
date: "2000-01-01"
weight : 04
chapter : false
pre : " <b> 5.4. </b> "
---

This Monitoring Setup phase activates and configures the three core log sources for threat detection. It involves enabling CloudTrail for comprehensive management and data events, activating GuardDuty to export security findings to the primary S3 bucket, and setting up VPC Flow Logs on your network to send all traffic metadata to the dedicated CloudWatch Log Group. This ensures a constant, centralized stream of log data is available for processing and automated response.


## Create CloudWatch Log Group

1.  **Open CloudWatch Console** → **Log Management** → **Create log group**
![alt text](</images/5-Workshop/Workshop pic/21 Open CloudWatch Console → Log groups → Create log group 5.4.png>)
2.  **Configure**:

      - **Log group name**: `/aws/incident-response/centralized-logs`
      - **Retention**: 90 days
      - **KMS key**: None
3.  **Click "Create"**
-----

## Enable AWS CloudTrail

1.  **Open CloudTrail Console** → **Trail** → **Create trail**
![alt text](</images/5-Workshop/Workshop pic/22 Open CloudTrail Console 5.4.png>)
2.  **Trail attributes**:

      - **Trail name**: `incident-responses-cloudtrail-ACCOUNT_ID-REGION`
      - **Storage location**: Use existing S3 bucket
      - **S3 bucket**: Choose your `incident-response-log-list-bucket-ACCOUNT_ID-REGION`
      - **Log file SSE-KMS encryption**: Disable
      - **Log file validation**: Enabled
      - Click next

3.  **Choose log events**:
      - **Events** Choose **Management events**, **Data events**
      - **Management events**: All (Read + Write)
      - **Data events**: S3 - Log all events
      - Click next till step 4 and **Create Trail**

4.  **Advanced event selectors: Exlcude log buckets**:
      - **Click the Trail then scroll down to Data Event and click Edit**
![alt text](</images/5-Workshop/Workshop pic/23 Advanced event selectors Exlcude log buckets.png>)
      - **Setup like picture with the under format**:
![alt text](</images/5-Workshop/Workshop pic/24 [Screenshot CloudTrail septup huhu] .png>)

-`arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/`

-`arn:aws:s3:::processed-guardduty-findings-ACCOUNT_ID-REGION/` 

-`arn:aws:s3:::processed-cloudtrail-logs-ACCOUNT_ID-REGION` 

-`arn:aws:s3:::athena-query-results-ACCOUNT_ID-REGION/`   

-`arn:aws:s3:::processed-cloudwatch-logs-ACCOUNT_ID-REGION/`

5.  **Save change**

-----

## Enable Amazon GuardDuty

1.  **Open GuardDuty Console** → **Get Started** → **Enable GuardDuty**

2.  **Configure settings**:

      - **Finding export frequency**: Update CWE and S3 every 15 minutes
      - **S3 export**: `incident-response-log-list-bucket-ACCOUNT_ID-REGION`
      - **KMS encryption**: Choose or create KMS key

-----

## Enable VPC Flow Logs

1.  **Open VPC Console** → **Your VPCs** → Select your VPC

2.  **Actions** → **Create flow log**

3.  **Configure**:

      - **Filter**: All
      - **Aggregation interval**: 10 minutes
      - **Destination**: CloudWatch Logs
      - **Log group**: `/aws/incident-response/centralized-logs`
      - **IAM role**: `FlowLogsIAMRole`
      - **Log format**: Default
  
4.  **Create flow log**

-----

## Enable VPC DNS Query Logging

### Configure Resolver Query Logging

1.  **Open the Amazon Route 53 Console**.
   
2.  In the left navigation pane, select **VPC Resolver** -> **Query logging**.

3.  Click **"Configure query logging"**.

4.  **Configure**:

    * **Name**: Enter a descriptive name, e.g., `IR-DNS-Query-Log-Config`.
    **Destination for query logs**: CloudWatch Logs log group
    * **Log group**: Select **"Existing log group"** and choose:
        * **`/aws/incident-response/centralized-logs`**

5.  Click **"Configure query logging"**.

-----


