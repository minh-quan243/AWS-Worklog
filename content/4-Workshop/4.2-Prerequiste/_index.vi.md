---
title : "Điều kiện tiên quyết"
date: "2000-01-01" 
weight: 2
chapter : false
pre : " <b> 4.2. </b> "
---

#### Yêu cầu

Trước khi bắt đầu workshop, hãy đảm bảo bạn đã chuẩn bị sẵn những điều sau:

- Một **tài khoản AWS** có quyền truy cập console
- Một **người dùng IAM** (IAM user) có đủ quyền (xem bên dưới)
- Một **kho lưu trữ GitHub** (GitHub repository) chứa mã nguồn frontend của Voice Summarizer (cho bước triển khai Amplify)
- Làm quen cơ bản với AWS Management Console

#### Quyền IAM

Người dùng IAM của bạn phải có quyền tạo và quản lý tất cả các dịch vụ được sử dụng trong workshop này. Hãy gắn (attach) policy sau cho người dùng hoặc role của bạn trước khi tiếp tục.
Policy phải bao gồm các dịch vụ và hành động sau:

| Dịch vụ | Các Hành động Yêu cầu |
|---|---|
| **EC2** | Tạo/xóa VPC, subnets, security groups, route tables, internet gateways, NAT gateways, endpoints, EC2 instances |
| **S3** | Tạo/xóa buckets, thêm/lấy/xóa objects, quản lý các bucket policies và cài đặt truy cập công khai |
| **DynamoDB** | Tạo/xóa bảng, đọc/ghi các mục (items) |
| **Lambda** | Tạo/xóa các hàm, thêm/xóa triggers, gọi (invoke) các hàm |
| **IAM** | Tạo/xóa roles và instance profiles, gắn (attach) policies, truyền (pass) roles |
| **Cognito** | Tạo/xóa user pools và app clients |
| **Amplify** | Tạo/xóa apps, branches, và các bản triển khai (deployments) |
| **Transcribe** | Bắt đầu và xem thông tin (describe) các công việc phiên âm |
| **CloudWatch / Logs** | Tạo log groups, đặt chính sách lưu giữ (retention policies) |

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VoiceSummarizerWorkshop",
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "s3:*",
        "dynamodb:*",
        "lambda:*",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:AttachRolePolicy",
        "iam:DetachRolePolicy",
        "iam:PutRolePolicy",
        "iam:DeleteRolePolicy",
        "iam:GetRole",
        "iam:PassRole",
        "iam:CreateInstanceProfile",
        "iam:DeleteInstanceProfile",
        "iam:AddRoleToInstanceProfile",
        "iam:RemoveRoleFromInstanceProfile",
        "cognito-idp:*",
        "amplify:*",
        "transcribe:*",
        "logs:CreateLogGroup",
        "logs:DeleteLogGroup",
        "logs:DescribeLogGroups",
        "logs:PutRetentionPolicy",
        "cloudwatch:*"
      ],
      "Resource": "*"
    }
  ]
}