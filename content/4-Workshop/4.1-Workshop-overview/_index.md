---
title : "Overview"
date: "2026-01-11" 
weight : 1 
chapter : false
pre : " <b> 4.1. </b> "
---

#### Project Overview

**Voice Summarizer** solves a common problem in modern organisations: valuable information locked inside audio recordings is practically inaccessible after the fact. Replaying hours of meetings to find a single decision or action item is impractical.

This platform automates the full pipeline — from raw audio upload to an interactive AI-powered knowledge interface — so any word spoken in any recorded session is instantly retrievable via natural language.

#### Core User Capabilities

**1. Automated Audio-to-Knowledge Pipeline**

Users upload an audio file (WAV, MP3, M4A) via a presigned S3 URL directly to cloud storage. An asynchronous worker pipeline picks up the upload, submits it to AWS Transcribe, waits for completion, chunks and embeds the transcript, and stores the vectors in a semantic index — all without user intervention.

**2. Conversational AI Assistant**

Each recording gets its own chat interface. Users ask questions in natural language; the system classifies the intent, selects the appropriate retrieval strategy, fetches the most relevant transcript segments, and generates a grounded answer from an LLM. Conversation memory persists across sessions.

**3. Recording Library & Dashboard**

A searchable library lists all processed recordings with AI-generated summaries, processing status, duration, and creation date.

#### System Architecture

The platform is deployed in **ap-southeast-1 (Singapore)** and uses a hybrid architecture combining serverless triggers with an EC2-hosted backend:

![Architecture Overview](/images/2-Proposal/AWS_Solution_Architecture.jpg)

**Traffic flow (numbered as in the diagram):**

- **(0)** User accesses the React frontend hosted on **AWS Amplify**
- **(1)** User authenticates via **Amazon Cognito**; JWT tokens are issued
- **(2–3)** Cognito post-confirmation trigger fires **Lambda** → provisions user record in **DynamoDB**
- **(4)** API calls from the frontend reach the **Application Load Balancer (ALB)** in the public subnet
- **(5–6)** ALB routes traffic through **VPC Gateway Endpoints** to the **EC2 instance** in the private subnet
- **(7)** EC2 (FastAPI + Celery + Redis) reads/writes recording status and conversation memory to **DynamoDB**
- **(8)** Audio files are uploaded directly to **S3 (Audio bucket)** via presigned URL
- **(9)** S3 event notification triggers the **audio-to-text Lambda**
- **(10)** Lambda starts an **AWS Transcribe** job
- **(11)** Transcribe writes the completed transcript to **S3 (Transcript bucket)**
- **(12)** EC2 Celery worker reads the transcript from S3
- **(13)** EC2 writes and reads the vector index from **S3 (Vectors bucket)**
- **(14–16)** Outbound LLM API calls from EC2 exit via **NAT Gateway → Internet Gateway → Internet**

#### Services You Will Configure

| Service | Purpose |
|---|---|
| Amazon VPC | Network isolation with public and private subnets |
| Internet Gateway | Inbound/outbound internet access for the public subnet |
| NAT Gateway | Outbound-only internet access for the private subnet (LLM API calls) |
| VPC Gateway Endpoints | Private connectivity to S3 and DynamoDB from the private subnet |
| Security Group | Firewall rules for EC2 |
| AWS S3 (×3) | Raw audio, transcripts, and vector embeddings |
| Amazon DynamoDB (×2) | Recording status table and user table |
| AWS Lambda (×2) | Audio transcription trigger and Cognito post-confirmation user provisioning |
| AWS EC2 | Backend compute: FastAPI API server, Celery worker, Redis |
| Application Load Balancer | Routes frontend API traffic to the private EC2 instance |
| Amazon Cognito | User pool with JWT-based authentication |
| AWS Amplify | Hosts and deploys the React frontend from GitHub |

#### Estimated Time

| Section | Estimated Duration |
|---|---|
| VPC & Networking | ~25 min |
| Storage (S3 + DynamoDB) | ~15 min |
| Lambda Functions | ~15 min |
| EC2 Setup | ~10 min |
| ALB Setup | ~10 min |
| Cognito Setup | ~10 min |
| Amplify Deployment | ~10 min |
| **Total** | **~95 min** |
