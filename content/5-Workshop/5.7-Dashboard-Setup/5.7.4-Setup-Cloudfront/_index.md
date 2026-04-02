---
title : "Cloudfront Setup"
date: "2000-01-01"
weight : 04
chapter : false
pre : " <b> 5.7.4. </b> "
---
In this guide, you will setup a Cloudfront for cache, routing and web accessing.

## Create Cloudfront Distribution

1. **Open the Cloudfront Console**
   - Navigate to https://console.aws.amazon.com/cloudfront/
   - Or: AWS Management Console → Services → Cloudfront

   ![Screenshot: Cloudfront Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create.png)

2. **Create Distribution**:
   - Click the **Create distribution** button
   - In distribution creation, use this setting:
     - Choose a plan: **Free plan**
     - Name: **Static Dashboard Website CloudFront**
     - Origin type: **Amazom S3**
     - S3 Origin: Choose the **static-dashboard-bucket**
     - Keep the rest like default
     - Enable security: Use this if you choose **free plan**
     - Review and click **Create distribution**

    ![Screenshot: Cloudfront Distribution create setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create_button.png)

    -----

    ![Screenshot: Cloudfront Distribution create setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create_step_1.png)

    -----

    ![Screenshot: Cloudfront Distribution create setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create_step_2.png)

    -----

    ![Screenshot: Cloudfront Distribution create setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create_step_3.png)

    -----

    ![Screenshot: Cloudfront Distribution create setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create_step_4.png)

    -----

    ![Screenshot: Cloudfront Distribution create setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create_step_5.png)

3. **General setting**:
   - After creation complete, on your Cloudfront General tab click on **Edit**
   - At the **Default root object** enter **index.html**
   - Description: **Static Dashboard Distribution**
   - Click **Save change**

    ![Screenshot: Cloudfront Distribution general setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_general_edit.png)

    -----

    ![Screenshot: Cloudfront Distribution general setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_root_object.png)

4. **Create API Gateway origin**:
   - Click **Origins** on the menu tabs
   - Then click **Create origin**
   - In orogin creation, use this setting:
     - Origin domain: choose **dashboard-api**
     - Protocol: **HTTPS only**
     - HTTPS port: **443**
     - Minimum Origin SSL protocol: **TLSv1.2**
     - Origin path: **/prod**
   - Click **Create origin**

    ![Screenshot: Cloudfront Distribution origin setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_origin_create.png)

    -----

    ![Screenshot: Cloudfront Distribution origin setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_origin_create_result.png)

5. **Create behaviors for API Gateway**:
   - Click **Behaviors** on the menu tabs
   - Then click **Create behavior**
   - In behavior creation, use this setting:
     - Path pattern: **/logs/\***
     - Origin and origin groups: choose **dashboard-api**
     - Leave the rest setting like default
     - Click **Create behavior**

    ![Screenshot: Cloudfront Distribution behavior setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_behavior_create.png)

    -----

    ![Screenshot: Cloudfront Distribution behavior setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_behavior_create_setting.png)

6. **Update S3 policy to work with Cloudfront**:
   - Click **Origins** on the menu tabs, choose the **s3-static-dashboard** origin name
   - Click **Edit**
   - At **Origin access controll** section press **Go to S3 bucket permissions**
   - Check if your S3 permission look like this, if don't then copy and paste it to your S3 permission (Change the `ACCOUNT_ID`, `ACCOUNT_REGION` and `CLOUDFRONT_ID` to your):

    ```Json
    {
        "Version": "2008-10-17",
        "Id": "PolicyForCloudFrontPrivateContent",
        "Statement": [
            {
                "Sid": "AllowCloudFrontServicePrincipal",
                "Effect": "Allow",
                "Principal": {
                    "Service": "cloudfront.amazonaws.com"
                },
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::s3-static-dashboard-[ACCOUNT_ID]-[ACCOUNT_REGION]/*",
                "Condition": {
                    "ArnLike": {
                        "AWS:SourceArn": "arn:aws:cloudfront::[ACCOUNT_ID]:distribution/[CLOUDFRONT_ID]"
                    }
                }
            }
        ]
    }
    ```
    - Click **Save change**

    ![Screenshot: S3 Bucket permission for Cloudfront](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_s3_origin_edit.png)

    -----

    ![Screenshot: S3 Bucket permission for Cloudfront](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_s3_permission.png)

    -----

    ![Screenshot: S3 Bucket permission for Cloudfront](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_s3_policy.png)

7. **Create error pages**:
   - Click **Error pages** on the menu tabs
   - Click **Create custom error page**
   - In custom error page creation, use this setting:
     - HTTP error code: **403: Forbident**
     - Error caching minimum TTL: **300**
     - Customize error response: **Yes**
     - Response page path: **/index.html**
     - HTTP Response code: **200: OK**
   - Repeat this for **404 code**

    ![Screenshot: Cloudfront Distribution error page setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_error_page_create.png)

    -----

    ![Screenshot: Cloudfront Distribution error page setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_error_page_403.png)