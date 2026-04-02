---
title : "Cognito Setup"
date: "2000-01-01"
weight : 05
chapter : false
pre : " <b> 5.7.5. </b> "
---
In this guide, you will create a Cognito user pool for dashboard login.

## Create Cognito User Pool

1. **Open the Amazon Cognito Console**
   - Navigate to https://console.aws.amazon.com/cognito/
   - Or: AWS Management Console → Services → Cognito

   ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_page.png)

2. **Create user pool**:
   - Click **Create user pool**
   - In user pool creation, use this setting:
     - Application type: **Single-page application (SPA)**
     - Application name: **dashboard-user-pool-client**
     - Options for sign-in identifiers: **Email** and **Username**
     - Self-registration: **Enable self-registration**
     - Required attributes for sign-up: **email**
     - Add a return URL: Go to Cloudfront, choose the one that you just created and copy the **Distribution domain name** and paste it here (Example: `https://d2bvvvpr6s4eyd.cloudfront.net`)
     - Click **Create user directory**
     - After create, scroll down and click **Go to overview**

    ![Screenshot: Cognito User pool create](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_get_cloudfront_url.png)

    -----

    ![Screenshot: Cognito User pool create](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_create.png)

3. **User pool App clients configuration**:
   - Select **App clients** on the left menu panel
   - Choose **dashboard-user-pool-client**
   - In **App client information** section, click **Edit**
   - Change the setting like the image below:
   - Click **Save change**

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_app_client_select.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_information_edit.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_information_setting.png)

4. **Managed login pages configuration**:
   - In **Managed login pages configuration** section, click **Edit**
   - Click **Add sign-out URL** at **Allowed sign-out URLs** section
   - Copy the URL on the **callbacks URL** and paste to **Allowed sign-out URLs**
   - Scroll down to **OpenID Connect scopes** add **Profile** to the scopes
   - Click **Save change**

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_managed_login.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_managed_login_setting.png)

5. **Create a user**:
   - On the left menu panel, select **User** option
   - Click **Create user**
   - Enter your user information
   - Click **Create user**

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_create_user_page.png)

    -----

    ![Screenshot: Cognito Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.5-cognito-setup/cognito_create_user_setting.png)