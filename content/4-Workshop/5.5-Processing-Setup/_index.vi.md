---
title : "Thiết lập xử lý"
date: "2000-01-01"
weight : 05
chapter : false
pre : " <b> 5.5. </b> "
---

Giai đoạn Thiết lập xử lý này xây dựng đường ống dữ liệu (data pipeline) cốt lõi để cấu trúc log thô và chuẩn bị chúng cho việc phân tích truy vấn. Giai đoạn này bắt buộc triển khai ba luồng Kinesis Data Firehose để đệm và phân phối CloudTrail và VPC logs đến các S3 buckets đích. Đồng thời, bạn sẽ cấu hình AWS Glue Database và bốn bảng Athena thông qua DDL để làm cho dữ liệu có cấu trúc có thể truy vấn được. Pipeline này dựa vào năm Lambda functions ETL được kích hoạt bởi S3 Event Notifications để thực hiện chuyển đổi dữ liệu cần thiết khi log đến.

-----
#### Nội dung

- [Tạo Kinesis Data Firehose Delivery Streams](5.5.1-create-kinesis-data-firehose/)
- [Tạo AWS Glue Database và Tables](5.5.2-create-aws-glue-database-and-tables/)
- [Tạo Lambda Functions - Xử lý ETL](5.5.3-create-lambda-function-etl-processing/)
