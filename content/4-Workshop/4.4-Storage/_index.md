---
title : "Storage Setup"
date: "2000-01-01"
weight: 4
chapter : false
pre : " <b> 4.4. </b> "
---

In this section you will create the three S3 buckets and two DynamoDB tables that form the persistent data layer of the Voice Summarizer platform.

| Resource | Name | Purpose |
|---|---|---|
| S3 Bucket | `one4allthing` (Audio + Transcript) | Stores raw audio uploads and AWS Transcribe output |
| S3 Bucket | *(your choice)* (Vectors) | Stores vector embeddings and semantic index |
| DynamoDB Table | `VoiceSummarizerHistory` | Per-recording processing status, progress, and conversation memory |
| DynamoDB Table | `User` | User records provisioned on Cognito sign-up |

#### Content

- [S3 Audio Bucket](4.4.1-s3-audio-bucket/)
- [S3 Vectors Bucket](4.4.2-s3-vectors-bucket/)
- [DynamoDB Tables](4.4.3-dynamodb/)
