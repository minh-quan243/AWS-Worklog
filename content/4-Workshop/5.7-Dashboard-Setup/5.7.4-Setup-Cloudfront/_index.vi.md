---
title : "Cài đặt Cloudfront"
date: "2000-01-01"
weight : 04
chapter : false
pre : " <b> 5.7.4. </b> "
---
Trong hướng dẫn này, bạn sẽ cài đặt một Cloudfront để cache, định tuyến và truy cập web.

## Tạo Cloudfront Distribution

1. **Mở Cloudfront Console**
   - Điều hướng tới https://console.aws.amazon.com/cloudfront/
   - Hoặc: AWS Management Console → Services → Cloudfront

   ![Screenshot: Cloudfront Console Homepage](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_create.png)

2. **Tạo Distribution**:
   - Nhấn nút **Create distribution**
   - Trong phần tạo distribution, sử dụng cài đặt này:
     - Chọn plan: **Free plan**
     - Name: **Static Dashboard Website CloudFront**
     - Origin type: **Amazom S3**
     - S3 Origin: Chọn **static-dashboard-bucket**
     - Giữ phần còn lại như mặc định
     - Bật security: Sử dụng cái này nếu bạn chọn **free plan**
     - Xem lại và nhấn **Create distribution**

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

3. **Cài đặt chung (General setting)**:
   - Sau khi tạo xong, trên tab General của Cloudfront nhấn vào **Edit**
   - Tại **Default root object** nhập **index.html**
   - Description: **Static Dashboard Distribution**
   - Nhấn **Save change**

    ![Screenshot: Cloudfront Distribution general setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_general_edit.png)

    -----

    ![Screenshot: Cloudfront Distribution general setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_root_object.png)

4. **Tạo API Gateway origin**:
   - Nhấn **Origins** trên các tab menu
   - Sau đó nhấn **Create origin**
   - Trong phần tạo origin, sử dụng cài đặt này:
     - Origin domain: chọn **dashboard-api**
     - Protocol: **HTTPS only**
     - HTTPS port: **443**
     - Minimum Origin SSL protocol: **TLSv1.2**
     - Origin path: **/prod**
   - Nhấn **Create origin**

    ![Screenshot: Cloudfront Distribution origin setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_origin_create.png)

    -----

    ![Screenshot: Cloudfront Distribution origin setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_origin_create_result.png)

5. **Tạo behaviors cho API Gateway**:
   - Nhấn **Behaviors** trên các tab menu
   - Sau đó nhấn **Create behavior**
   - Trong phần tạo behavior, sử dụng cài đặt này:
     - Path pattern: **/logs/\***
     - Origin và origin groups: chọn **dashboard-api**
     - Để các cài đặt còn lại như mặc định
     - Nhấn **Create behavior**

    ![Screenshot: Cloudfront Distribution behavior setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_behavior_create.png)

    -----

    ![Screenshot: Cloudfront Distribution behavior setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_behavior_create_setting.png)

6. **Cập nhật S3 policy để hoạt động với Cloudfront**:
   - Nhấn **Origins** trên các tab menu, chọn tên origin **s3-static-dashboard**
   - Nhấn **Edit**
   - Tại phần **Origin access controll** nhấn **Go to S3 bucket permissions**
   - Kiểm tra xem quyền S3 của bạn có giống thế này không, nếu không hãy copy và paste nó vào quyền S3 của bạn (Thay đổi `ACCOUNT_ID`, `ACCOUNT_REGION` và `CLOUDFRONT_ID` thành của bạn):

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
    - Nhấn **Save change**

    ![Screenshot: S3 Bucket permission for Cloudfront](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_s3_origin_edit.png)

    -----

    ![Screenshot: S3 Bucket permission for Cloudfront](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_s3_permission.png)

    -----

    ![Screenshot: S3 Bucket permission for Cloudfront](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_s3_policy.png)

7. **Tạo error pages**:
   - Nhấn **Error pages** trên các tab menu
   - Nhấn **Create custom error page**
   - Trong phần tạo custom error page, sử dụng cài đặt này:
     - HTTP error code: **403: Forbident**
     - Error caching minimum TTL: **300**
     - Customize error response: **Yes**
     - Response page path: **/index.html**
     - HTTP Response code: **200: OK**
   - Lặp lại bước này cho **404 code**

    ![Screenshot: Cloudfront Distribution error page setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_error_page_create.png)

    -----

    ![Screenshot: Cloudfront Distribution error page setting](/images/5-Workshop/5.7-Dashboard-setup/5.7.4-cloudfront-setup/cloudfront_error_page_403.png)