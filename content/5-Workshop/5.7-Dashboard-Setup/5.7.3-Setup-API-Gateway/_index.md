---
title : "API Gateway Setup"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.7.3. </b> "
---
In this guide, you will setup an API Gateway to route api call from dashboard to Lambda.

## Create API Gateway

1. **Open the API Gateway Console**
   - Navigate to https://console.aws.amazon.com/apigateway/
   - Or: AWS Management Console → Services → API Gateway

   ![Screenshot: API Gateway Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_page.png)

2. **Create API**:
   - Click **Create API**
   - Choose **REST API** and click **Build**
   - Use this setting for creation:
     - Choose **New API**
     - Name: **dashboard-api**
     - API endpoint type: **Regional**
     - Security policy: **SecurityPolicy_TLS13_1_3_2025_09**
     - Endpoint access mode: **Basic**
     - IP address type: **IPv4**

    ![Screenshot: API Gateway creation setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_gateway_create_setting.png)

3. **Create Resources**:
    - Enable CORS for the root resource
    - Click **Create resource** and name it **logs**
    - Then click on **/logs** resource that just created and click **Create Resource** to create child resource of **/logs**
    - Name it **cloudtrail** and enable CORS
    - Repeat this three more times for **eni_logs**, **guardduty** and **vpc**

    ![Screenshot: API Gateway resource list](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_resource_create.png)

4. **Create methods**:
    - Click on **/cloudtrail** that just created and click **Cretae method**
    - In method creation, use this setting:
      - Method type: **GET**
      - Intergration type: **Lambda function**
      - Enable **Lambda proxy intergration** choose **Buffered**
      - Lambda function: select your region search for **dashboard-query** and choose it
      - Timout: **29000**

    - Repeat this three more time for **eni_logs**, **guardduty** and **vpc**

    ![Screenshot: API Gateway method list](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_method_create.png)

    -----

    ![Screenshot: API Gateway method list](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_result.png)

5. **Deploy API**:
   - Click the **Deploy API** on the right corner

    ![Screenshot: API Gateway Deploy button](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_deploy.png)

    - In deploy API, use this setting:
      - Stage: **New stage**
      - Name: **prod**
      - Click **Deploy**
    ![Screenshot: API Gateway Deploy setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_deploy_setting.png)

    -----

    ![Screenshot: API Gateway Deploy result](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_deploy_result.png)
