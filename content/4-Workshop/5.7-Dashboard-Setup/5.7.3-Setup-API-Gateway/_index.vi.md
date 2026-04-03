---
title : "Cài đặt API Gateway"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.7.3. </b> "
---
Trong hướng dẫn này, bạn sẽ cài đặt một API Gateway để định tuyến cuộc gọi api từ dashboard tới Lambda.

## Tạo API Gateway

1. **Mở API Gateway Console**
   - Điều hướng tới https://console.aws.amazon.com/apigateway/
   - Hoặc: AWS Management Console → Services → API Gateway

   ![Screenshot: API Gateway Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_page.png)

2. **Tạo API**:
   - Nhấn **Create API**
   - Chọn **REST API** và nhấn **Build**
   - Sử dụng cài đặt này để tạo:
     - Chọn **New API**
     - Name: **dashboard-api**
     - API endpoint type: **Regional**
     - Security policy: **SecurityPolicy_TLS13_1_3_2025_09**
     - Endpoint access mode: **Basic**
     - IP address type: **IPv4**

    ![Screenshot: API Gateway creation setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_gateway_create_setting.png)

3. **Tạo Resources**:
    - Bật CORS cho root resource
    - Nhấn **Create resource** và đặt tên là **logs**
    - Sau đó nhấn vào tài nguyên **/logs** vừa tạo và nhấn **Create Resource** để tạo tài nguyên con của **/logs**
    - Đặt tên là **cloudtrail** và bật CORS
    - Lặp lại ba lần nữa cho **eni_logs**, **guardduty** và **vpc**

    ![Screenshot: API Gateway resource list](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_resource_create.png)

4. **Tạo methods**:
    - Nhấn vào **/cloudtrail** vừa tạo và nhấn **Create method**
    - Trong phần tạo method, sử dụng cài đặt này:
      - Method type: **GET**
      - Intergration type: **Lambda function**
      - Bật **Lambda proxy intergration** chọn **Buffered**
      - Lambda function: chọn region của bạn, tìm kiếm **dashboard-query** và chọn nó
      - Timout: **29000**

    - Lặp lại ba lần nữa cho **eni_logs**, **guardduty** và **vpc**

    ![Screenshot: API Gateway method list](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_method_create.png)

    -----

    ![Screenshot: API Gateway method list](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_result.png)

5. **Triển khai API (Deploy API)**:
   - Nhấn **Deploy API** ở góc phải

    ![Screenshot: API Gateway Deploy button](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_deploy.png)

    - Trong phần deploy API, sử dụng cài đặt này:
      - Stage: **New stage**
      - Name: **prod**
      - Nhấn **Deploy**
    ![Screenshot: API Gateway Deploy setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_deploy_setting.png)

    -----

    ![Screenshot: API Gateway Deploy result](/images/5-Workshop/5.7-Dashboard-setup/5.7.3-api-gateway-setup/api_deploy_result.png)