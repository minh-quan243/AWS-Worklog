---
title : "Cài đặt Cognito"
date: "2000-01-01"
weight : 05
chapter : false
pre : " <b> 5.7.5. </b> "
---
Trong hướng dẫn này, bạn sẽ tạo một Cognito user pool để đăng nhập dashboard.

## Tạo Cognito User Pool

1. **Mở Amazon Cognito Console**
   - Điều hướng tới https://console.aws.amazon.com/cognito/
   - Hoặc: AWS Management Console → Services → Cognito

   ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_page.png)

2. **Tạo user pool**:
   - Nhấn **Create user pool**
   - Trong phần tạo user pool, sử dụng cài đặt này:
     - Application type: **Single-page application (SPA)**
     - Application name: **dashboard-user-pool-client**
     - Options for sign-in identifiers: **Email** và **Username**
     - Self-registration: **Enable self-registration**
     - Required attributes for sign-up: **email**
     - Add a return URL: Vào Cloudfront, chọn cái bạn vừa tạo và copy **Distribution domain name** và dán vào đây (Ví dụ: `https://d2bvvvpr6s4eyd.cloudfront.net`)
     - Nhấn **Create user directory**
     - Sau khi tạo, cuộn xuống và nhấn **Go to overview**

    ![Screenshot: Cognito User pool create](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_get_cloudfront_url.png)

    -----

    ![Screenshot: Cognito User pool create](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_create.png)

3. **Cấu hình User pool App clients**:
   - Chọn **App clients** trên menu bên trái
   - Chọn **dashboard-user-pool-client**
   - Trong phần **App client information**, nhấn **Edit**
   - Thay đổi cài đặt như hình bên dưới:
   - Nhấn **Save change**

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_app_client_select.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_information_edit.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_information_setting.png)

4. **Cấu hình Managed login pages**:
   - Trong phần **Managed login pages configuration**, nhấn **Edit**
   - Nhấn **Add sign-out URL** tại phần **Allowed sign-out URLs**
   - Copy URL trên **callbacks URL** và dán vào **Allowed sign-out URLs**
   - Cuộn xuống **OpenID Connect scopes** thêm **Profile** vào scopes
   - Nhấn **Save change**

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_managed_login.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_managed_login_setting.png)

5. **Tạo một user**:
   - Trên menu bên trái, chọn tùy chọn **User**
   - Nhấn **Create user**
   - Nhập thông tin người dùng của bạn
   - Nhấn **Create user**

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_create_user_page.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_create_user_setting.png)