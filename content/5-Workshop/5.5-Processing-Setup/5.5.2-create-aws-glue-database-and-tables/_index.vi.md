---
title : "Tạo AWS Glue Database và Tables"
date: "2000-01-01"
weight : 02
chapter : false
pre : " <b> 5.5.2. </b> "
---

## Tạo AWS Glue Database và Tables

### Tạo Database

1.  **Mở Glue Console** → **Databases** → **Add database**

2.  **Database name**: `security_logs`

3.  **Create database**

### Tạo Tables (Sử dụng Athena DDL)

1.  **Mở Athena Console**

2.  **Đặt vị trí lưu kết quả truy vấn (query result location)**: `s3://athena-query-results-ACCOUNT_ID-REGION/`

3.  **Chọn database**: `security_logs`

#### Tạo bảng processed\_cloudtrail

**Chạy DDL này trong Athena** (thay thế `ACCOUNT_ID` và `REGION`):

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS security_logs.processed_cloudtrail (
  `eventtime` string,
  `eventname` string,
  `eventsource` string,
  `awsregion` string,
  `sourceipaddress` string,
  `useragent` string,
  `useridentity` struct<
    type:string,
    invokedby:string,
    principalid:string,
    arn:string,
    accountid:string,
    accesskeyid:string,
    username:string,
    sessioncontext:struct<
      attributes:map<string,string>,
      sessionissuer:struct<
        type:string,
        principalid:string,
        arn:string,
        accountid:string,
        username:string
      >
    >,
    inscopeof:struct<
      issuertype:string,
      credentialsissuedto:string
    >
  >,
  `requestparameters` string,
  `responseelements` string,
  `resources` array<struct<arn:string,type:string>>,
  `recipientaccountid` string,
  `serviceeventdetails` string,
  `errorcode` string,
  `errormessage` string,
  `hour` string,
  `usertype` string,
  `username` string,
  `isconsolelogin` boolean,
  `isfailedlogin` boolean,
  `isrootuser` boolean,
  `isassumedrole` boolean,
  `ishighriskevent` boolean,
  `isprivilegedaction` boolean,
  `isdataaccess` boolean,
  `target_bucket` string,
  `target_key` string,
  `target_username` string,
  `target_rolename` string,
  `target_policyname` string,
  `new_access_key` string,
  `new_instance_id` string,
  `target_group_id` string,
  `identity_principalid` string
)
PARTITIONED BY (
  `date` string
)
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = '1'
)
LOCATION 's3://processed-cloudtrail-logs-ACCOUNT_ID-REGION/processed-cloudtrail/'
TBLPROPERTIES (
  'projection.enabled' = 'true',
  'projection.date.type' = 'date',
  'projection.date.format' = 'yyyy-MM-dd',
  'projection.date.range' = '2025-01-01,NOW',
  'projection.date.interval' = '1',
  'projection.date.interval.unit' = 'DAYS',
  'storage.location.template' = 's3://processed-cloudtrail-logs-ACCOUNT_ID-REGION/processed-cloudtrail/date=${date}/',
  'classification' = 'json',
  'compressionType' = 'gzip'
);
```

#### Tạo bảng processed\_guardduty

Chạy DDL này trong Athena:

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS security_logs.processed_guardduty (
  `finding_id` string,
  `finding_type` string,
  `title` string,
  `severity` double,
  `account_id` string,
  `region` string,
  `created_at` string,
  `event_last_seen` string,
  `remote_ip` string,
  `remote_port` int,
  `connection_direction` string,
  `protocol` string,
  `dns_domain` string,
  `dns_protocol` string,
  `scanned_ip` string,
  `scanned_port` int,
  `aws_api_service` string,
  `aws_api_name` string,
  `aws_api_caller_type` string,
  `aws_api_error` string,
  `aws_api_remote_ip` string,
  `target_resource_arn` string,
  `instance_id` string,
  `instance_type` string,
  `image_id` string,
  `instance_tags` string,
  `resource_region` string,
  `access_key_id` string,
  `principal_id` string,
  `user_name` string,
  `s3_bucket_name` string,
  `service_raw` string,
  `resource_raw` string,
  `metadata_raw` string
)
PARTITIONED BY (
  `date` string
)
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = '1'
)
LOCATION 's3://processed-guardduty-findings-ACCOUNT_ID-REGION/processed-guardduty/'
TBLPROPERTIES (
  'classification' = 'json',
  'compressionType' = 'gzip',
  'projection.enabled' = 'true',
  'projection.date.type' = 'date',
  'projection.date.range' = '2025-01-01,NOW',
  'projection.date.format' = 'yyyy-MM-dd',
  'projection.date.interval' = '1',
  'projection.date.interval.unit' = 'DAYS',
  'storage.location.template' = 's3://processed-guardduty-findings-ACCOUNT_ID-REGION/processed-guardduty/date=${date}/'
);
```

#### Tạo bảng vpc\_logs

Chạy DDL này trong Athena:

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS security_logs.vpc_logs (
  `version` string,
  `account_id` string,
  `region` string,
  `vpc_id` string,
  `query_timestamp` string,
  `query_name` string,
  `query_type` string,
  `query_class` string,
  `rcode` string,
  `answers` string,
  `srcaddr` string,
  `srcport` int,
  `transport` string,
  `srcids_instance` string,
  `timestamp` string
)
PARTITIONED BY (
  `date` string
)
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = '1',
  'ignore.malformed.json' = 'true'
)
LOCATION 's3://processed-cloudwatch-logs-ACCOUNT_ID-REGION/vpc-logs/'
TBLPROPERTIES (
  'projection.enabled' = 'true',
  'projection.date.type' = 'date',
  'projection.date.format' = 'yyyy-MM-dd',
  'projection.date.range' = '2025-01-01,NOW',
  'projection.date.interval' = '1',
  'projection.date.interval.unit' = 'DAYS',
  'storage.location.template' = 's3://processed-cloudwatch-logs-ACCOUNT_ID-REGION/vpc-logs/date=${date}/',
  'classification' = 'json',
  'compressionType' = 'gzip'
);
```

#### Tạo bảng eni\_flow\_logs

Chạy DDL này trong Athena:

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS security_logs.eni_flow_logs (
  `version` int,
  `account_id` string,
  `interface_id` string,
  `srcaddr` string,
  `dstaddr` string,
  `srcport` int,
  `dstport` int,
  `protocol` int,
  `packets` bigint,
  `bytes` bigint,
  `start_time` bigint,
  `end_time` bigint,
  `action` string,
  `log_status` string,
  `timestamp_str` string
)
PARTITIONED BY (
  `date` string
)
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = '1'
)
LOCATION 's3://processed-cloudwatch-logs-ACCOUNT_ID-REGION/eni-flow-logs/'
TBLPROPERTIES (
  'projection.enabled' = 'true',
  'projection.date.type' = 'date',
  'projection.date.format' = 'yyyy-MM-dd',
  'projection.date.range' = '2025-01-01,NOW',
  'projection.date.interval' = '1',
  'projection.date.interval.unit' = 'DAYS',
  'storage.location.template' = 's3://processed-cloudwatch-logs-ACCOUNT_ID-REGION/eni-flow-logs/date=${date}/',
  'classification' = 'json',
  'compressionType' = 'gzip'
);
```
