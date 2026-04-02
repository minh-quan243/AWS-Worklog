---
title : "Processing Setup"
date: "2000-01-01"
weight : 05
chapter : false
pre : " <b> 5.5. </b> "
---

This Processing Setup phase establishes the core data pipeline for structuring raw logs and preparing them for queryable analysis. It mandates the deployment of three Kinesis Data Firehose streams for buffering and delivering CloudTrail and VPC logs to target S3 buckets. Concurrently, you will configure the AWS Glue Database and four Athena tables via DDL to make the structured data queryable. This pipeline relies on five ETL Lambda functions triggered by S3 Event Notifications to perform the necessary data transformation upon log arrival.

-----
#### Content

- [Create Kinesis Data Firehose Delivery Streams](5.5.1-create-kinesis-data-firehose/)
- [Create AWS Glue Database and Tables](5.5.2-create-aws-glue-database-and-tables/)
- [Create Lambda Functions - ETL Processing](5.5.3-create-lambda-function-etl-processing/)
