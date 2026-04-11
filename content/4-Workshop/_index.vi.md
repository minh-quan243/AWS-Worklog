---
title: "Workshop"
date: "2000-01-01"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# Voice Summarizer — Nền tảng Trí tuệ Âm thanh được hỗ trợ bởi AI

#### Tổng quan

Hướng dẫn này cung cấp quy trình từng bước, hoàn chỉnh để triển khai nền tảng Voice Summarizer trên AWS. Hệ thống này tận dụng **Amazon Cognito**, **AWS Amplify**, **S3**, **DynamoDB**, **Lambda** và **AWS Transcribe** cho quy trình xử lý trí tuệ âm thanh, với phần backend được lưu trữ trên **EC2** bên trong một **VPC** và được đưa ra ngoài thông qua **Application Load Balancer**. Người dùng có thể tải lên các bản ghi âm — các cuộc họp, bài giảng, phỏng vấn và cuộc gọi — những tệp này sẽ tự động được phiên âm, lập chỉ mục ngữ nghĩa và cho phép truy vấn thông qua giao diện AI đàm thoại do **LiteLLM** hỗ trợ.

#### Nội dung

1. [Tổng quan Workshop](4.1-Workshop-overview/)
2. [Điều kiện Tiên quyết](4.2-Prerequisites/)
3. [Mạng (Networking)](4.3-Networking/)
4. [Thiết lập Lưu trữ](4.4-Storage/)
5. [Các Hàm Lambda](4.5-Lambda/)
6. [Thiết lập EC2](4.6-EC2/)
7. [Application Load Balancer](4.7-ALB/)
8. [Cognito User Pool](4.8-Cognito/)
9. [Triển khai Amplify](4.9-Amplify/)
10. [Dọn dẹp](4.10-Cleanup/)