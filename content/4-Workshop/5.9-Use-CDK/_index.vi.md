---
title : "Sử dụng CDK"
date: "2000-01-01"
weight : 09
chapter : false
pre : " <b> 5.9. </b> "
---
## Tổng quan
Chúng tôi đã cung cấp CDK stack để tạo toàn bộ cơ sở hạ tầng cần thiết cho workshop này.

Để lấy các file, vui lòng truy cập [Github Link](https://github.com/AWS-Incident-Response-Automation-CDK/aws-incident-response-automation-cdk) và clone hoặc tải xuống tất cả các file về một thư mục.

## Hướng dẫn cài đặt

Trước khi triển khai CDK stack, bạn phải cấu hình môi trường cục bộ của mình để xác thực với tài khoản AWS bằng **AWS Command Line Interface (CLI)**.

1.  **Cài đặt AWS CLI**.
2.  **Lấy Credentials:** Bạn cần một **Access Key ID** và một **Secret Access Key** từ một IAM user có quyền deployment.
3.  **Chạy lệnh cấu hình:** Mở terminal và chạy lệnh `aws configure`.
    ```bash
    $ aws configure
    ```
    Khi được nhắc, nhập credentials và các cài đặt mong muốn. **Default region name** nên khớp với region nơi bạn định triển khai stack (ví dụ: `ap-southeast-1`):

    | Prompt | Example Value |
    | :--- | :--- |
    | `AWS Access Key ID` | `AKIA...` |
    | `AWS Secret Access Key` | `wJalr...` |
    | `Default region name` | `ap-southeast-1` |
    | `Default output format` | `json` |

4.  **Xác minh cấu hình:** Kiểm tra thiết lập bằng cách lấy user identity. Kết quả thành công xác nhận bạn đã xác thực.
    ```bash
    $ aws sts get-caller-identity
    ```

---
## Điều kiện tiên quyết

Đảm bảo các công cụ và dịch vụ sau đã được cài đặt và cấu hình trên hệ thống của bạn:

1.  **Python 3.8+ và pip:** Cần thiết để thực thi ứng dụng CDK và build Lambda function assets.
2.  **Node.js và npm:** Cần thiết để chạy AWS CDK CLI và build React dashboard.
3.  **AWS CDK Toolkit:** Cài đặt CDK CLI global:
    ```bash
    $ npm install -g aws-cdk
    ```
---

## Thiết lập môi trường Python

Định nghĩa cơ sở hạ tầng được viết bằng **Python**. Một **virtual environment** chuyên dụng được sử dụng để quản lý các dependencies của dự án.

1.  **Tạo Virtual Environment**:
    ```bash
    $ python -m venv .venv
    ```
2.  **Kích hoạt Virtual Environment**:

    | Operating System | Command |
    | :--- | :--- |
    | **macOS / Linux** | `source .venv/bin/activate` |
    | **Windows (Command Prompt)** | `.venv\Scripts\activate.bat` |
    | **Windows (PowerShell)** | `.venv\Scripts\Activate.ps1` |

3.  **Cài đặt Python Dependencies**:
    ```bash
    $ pip install -r requirements.txt
    ```
---

## Bước build dashboard

**Tại vị trí thư mục dự án, kiểm tra bên trong thư mục `react`. Nếu thư mục `dist` đã tồn tại, bạn không cần phải build. Nếu chưa, vui lòng làm theo các bước dưới đây.**
Nếu bạn đang dùng cmd sử dụng lệnh này để di chuyển vào thư mục `react`:
```
$ cd react
```
Và sử dụng lệnh này để liệt kê tất cả nội dung trong `react`:
```
$ ls
```

## Điều kiện tiên quyết
Đảm bảo bạn đã cài đặt **Node.js** và **npm**. Bạn có thể kiểm tra phiên bản hiện tại bằng cách chạy:
```
$ npm --version
```
Nếu lệnh không được nhận diện, vui lòng tải và cài đặt Node.js từ [nodejs.org](https://nodejs.org/en)

## Cài đặt dependencies
Chạy lệnh sau để cài đặt tất cả các thư viện cần thiết:
```
$ npm install
```

## Build Project
Sau khi cài đặt hoàn tất, chạy lệnh build:
```
$ npm run build
```
Sau khi hoàn tất, một thư mục dist sẽ được tạo ra chứa index.html và thư mục assets.

## Cấu hình Deployment Context

Stack sử dụng các biến context (context variables). Các biến này được đọc từ `cdk.context.json` hoặc cung cấp qua cờ (flags) dòng lệnh.

| Variable Name | Description | Required if functionality is desired | Default Value (in `cdk.context.json`) |
| :--- | :--- | :--- | :--- |
| `vpc_ids` | Danh sách các VPC IDs cho Flow Logs và DNS Query Logging. | Có | `[]` |
| `alert_email` | Danh sách các địa chỉ email cho thông báo cảnh báo (yêu cầu SES). | Có | `[]` |
| `sender_email` | Địa chỉ email người gửi SES đã xác thực. | Có (nếu `alert_email` được thiết lập) | `""` |
| `slack_webhook_url` | Slack webhook URL để gửi cảnh báo. | Không | `""` |

**Ví dụ**
```json
{
    "vpc_ids": [
        "vpc-a1b2c3d4e5f6g7h8i"
    ],
    "alert_email": [
        "admin@example.com"
    ],
    "sender_email": "alerts@your-domain.com",
    "slack_webhook_url": ""
}
```

## Triển khai Stacks (Deploy)

**Trước khi xử lý tiếp, nếu đang ở trong thư mục **/react**, nhập lệnh này để quay lại thư mục chính:**
```
$ cd..
```

1.  **CDK Bootstrapping:** Nếu bạn chưa từng sử dụng **AWS CDK** trong tài khoản AWS và region mục tiêu trước đây, chạy lệnh bootstrap một lần để cung cấp các tài nguyên cần thiết (ví dụ: S3 deployment bucket).
    ```bash
    $ cdk bootstrap
    ```

2.  **(Tùy chọn) Synthesize và Diff:** Xem lại các thay đổi CloudFormation được đề xuất trước khi deployment:
    ```bash
    $ cdk synth --all
    $ cdk diff --all
    ```

3.  **Execute Deployment:** Chạy lệnh deployment và phê duyệt bất kỳ **thay đổi bảo mật IAM** nào được yêu cầu khi được nhắc.
    ```bash
    $ cdk deploy --all
    ```

Việc deployment hoàn tất khi CDK CLI báo cáo thành công cho stack: `AwsIncidentResponseAutomationCdkStack` và `DashboardCdkStack` 

## LƯU Ý QUAN TRỌNG:
- Sau khi deployment hoàn tất, bạn nên xác minh email trong SES.
- Tạo một user trong Cognito để có thể đăng nhập vào Dashboard.
- Truy cập Security Group và xóa quy tắc outbound mặc định khỏi QuarantineSecurityGroup