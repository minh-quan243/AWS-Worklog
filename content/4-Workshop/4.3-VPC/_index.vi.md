---
title : "Networking"
date: "2000-01-01"
weight: 3
chapter : false
pre : " <b> 4.3. </b> "
---

Trong phần này, bạn sẽ xây dựng toàn bộ nền tảng mạng cho hệ thống Voice Summarizer, sau đó kết nối tên miền tùy chỉnh của bạn với các endpoint của nền tảng bằng Amazon Route 53.

Hai phần phụ này phải được hoàn thành theo thứ tự — các bản ghi bí danh (alias records) của Route 53 trỏ đến cơ sở hạ tầng (ALB, Amplify) được tạo ra trong các bước thiết lập VPC và ALB.

| Phần phụ | Những gì bạn sẽ tạo |
|---|---|
| Thiết lập VPC | VPC, các subnet công khai/riêng tư (public/private subnets), Internet Gateway, NAT Gateway, bảng định tuyến (route tables), Gateway Endpoints (S3 + DynamoDB), Nhóm bảo mật (Security Group) |
| Route 53 DNS | Public Hosted Zone, ủy quyền bản ghi NS (NS record delegation) tại nhà đăng ký của bạn, bản ghi bí danh A (alias A record) trỏ đến ALB |

#### Nội dung

- [Thiết lập VPC](4.3.1-vpc/)
- [Route 53 DNS](4.3.2-route53/)