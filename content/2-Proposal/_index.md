---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

![AWS Logo](/images/2-Proposal/AWSLogo.png)

# Proposal Link: [Proposal](https://docs.google.com/document/d/1Y2f0fEwtpavgm4-i7ZciMh5AVfeVkyjh/edit)

# Voice Summarizer — The Archivist
## AI-Powered Audio Intelligence Platform

**Team Members:**

| Full Name             | Student ID | Role        |
| --------------------- | ---------- | ----------- |
| Phan Minh Nhật Trường | SE196744   | Team Leader |
| Nguyễn Minh Quân      | SE193488   | Member      |
| Phạm Đăng Khoa        | SE192211   | Member      |
| Nguyễn Bá Tùng Dương  | SE193510   | Member      |
| Trịnh Thanh An        | SE193956   | Member      |

**AWS Services:** `Amplify` · `Cognito` · `Lambda` · `S3` · `DynamoDB` · `Transcribe` · `EC2`

---

### 1. Executive Summary

Voice Summarizer is an end-to-end AI platform that transforms audio recordings — meetings, lectures, interviews, and calls — into searchable, queryable knowledge bases. Users upload an audio file (WAV, MP3, M4A); the system automatically transcribes it, indexes it semantically, and exposes a conversational AI interface through which users can ask questions in natural language and receive precise, context-grounded answers.

The platform is built on a hybrid AWS architecture deployed in the Singapore region (ap-southeast-1). The frontend is hosted on AWS Amplify; the backend (FastAPI + Celery + Redis) runs on an EC2 instance in a private subnet, fronted by an Application Load Balancer (ALB) in a public subnet. A NAT Gateway provides the EC2 instance with outbound internet access for external LLM API calls. AWS Transcribe handles speech-to-text, triggered by a Lambda function on S3 object creation; a second Lambda provisions DynamoDB user records on Cognito sign-up. Three dedicated S3 buckets store raw audio, transcripts, and vector data. VPC Gateway Endpoints connect the private subnet to S3 and DynamoDB without traversing the public internet. LLM calls are routed through LiteLLM for model-agnostic flexibility, and access is secured via Amazon Cognito with JWT authentication.

---

### 2. Problem Statement

#### What's the Problem?

Organisations conduct thousands of hours of recorded meetings, client calls, training sessions, and interviews every year. Despite this volume, the information inside those recordings is effectively dark data — difficult to search, summarise, or act upon without listening from start to finish. Existing pain points include:

- No fast way to locate a specific decision, figure, or name mentioned in a long recording
- Manual note-taking is inconsistent, incomplete, and time-consuming
- Sharing insights from recordings requires re-listening and manual summarisation
- Compliance and audit use cases require retrievable evidence from recorded sessions

#### The Solution

Voice Summarizer solves these problems by automating the full pipeline from raw audio to an interactive, AI-powered knowledge interface. The platform provides three core capabilities:

1. **Automated Audio-to-Knowledge Pipeline** — users upload an audio file via a presigned S3 URL; an asynchronous Celery worker transcribes it via AWS Transcribe (triggered by a Lambda function), then chunks, embeds, and indexes the transcript into a semantic vector store — all without user intervention.
2. **Conversational AI Assistant** — each recording gets its own chat interface backed by a RAG question router that classifies query intent and selects the optimal retrieval strategy (factual, comprehensive, topic summary, full summary, or off-topic), generating hallucination-resistant answers grounded in the transcript.
3. **Recording Library & Dashboard** — a searchable library lists all processed recordings with status indicators, AI-generated short summaries, duration, and creation date.

#### Benefits and Return on Investment

Voice Summarizer eliminates the manual overhead of replaying, note-taking, and summarising audio recordings. Key benefits include:

- **Immediate productivity gains** — any fact, decision, or action item from any recording is retrievable in seconds
- **Reduced compliance risk** — full transcripts and audit trails are stored and queryable
- **Knowledge retention** — institutional knowledge captured in recordings is no longer lost after the meeting ends
- **Predictable infrastructure cost** — a fixed-size EC2 instance provides consistent compute capacity, with variable costs limited to Transcribe usage and data transfer

---

### 3. Solution Architecture

The system is deployed in AWS ap-southeast-1 (Singapore) and composed of eight loosely coupled layers:

![AWS Architecture](/images/2-Proposal/AWSWorkshopArchitecture-Final.png)

| Layer | Technology | Responsibility |
|---|---|---|
| Frontend Hosting | AWS Amplify | Serves the React + Vite SPA; manages CDN and HTTPS |
| Auth | AWS Cognito | User registration, JWT token management, post-confirmation Lambda trigger |
| Network | VPC · ALB · NAT Gateway · Internet Gateway · Gateway Endpoints | Public/private subnet isolation; routes traffic to EC2; enables secure S3/DynamoDB access from private subnet |
| Compute | AWS EC2 (private subnet) | Runs FastAPI API server, Celery worker, and Redis broker |
| Transcription | AWS Transcribe + Lambda | Speech-to-text; Lambda (`audio2text_lambda.py`) triggered by S3 Audio object creation |
| Vector Store | Sentence Transformers + S3 Vectors bucket | Embedding, chunking, semantic search; vectors persisted to dedicated S3 bucket |
| LLM Layer | LiteLLM (model-agnostic) | Abstraction over OpenAI, Anthropic, Bedrock; outbound calls via NAT Gateway |
| Storage | S3 (Audio) · S3 (Transcript) · S3 (Vectors) · DynamoDB | Three dedicated S3 buckets; DynamoDB for status, metadata, and conversation memory |

#### AWS Services Used

- **AWS Amplify**: Hosts the React + Vite frontend (step 0 in diagram). Users access the application directly through Amplify's managed CDN endpoint. Cognito JWT tokens are obtained here (step 1) and passed with all subsequent API calls.
- **Amazon Cognito**: Manages user registration, login, and JWT session management. A post-confirmation trigger fires a Lambda function (step 2) that provisions a new user record in DynamoDB (step 3).
- **AWS Lambda (user_creation_db.py)**: Cognito post-confirmation trigger; creates the user's DynamoDB entry on first sign-up (steps 2–3).
- **Application Load Balancer (ALB)**: Sits in the public subnet and receives all API traffic from Amplify (step 4). Routes requests through Gateway Endpoints into the private subnet (step 5), terminating TLS and distributing load to the EC2 target.
- **VPC Gateway Endpoints**: Deployed in the private subnet, they allow the EC2 instance to reach S3 and DynamoDB over the AWS private network without traversing the NAT Gateway (steps 5–6), improving security and reducing NAT data-processing costs.
- **AWS EC2 (private subnet)**: The core compute node. Runs the FastAPI API server, Celery worker, and Redis broker as co-located processes. Handles all business logic: presigned URL generation, RAG Q&A, vectorisation, and status management. Accesses DynamoDB (step 7), S3 Vectors (step 13), and S3 Transcript (step 12) via Gateway Endpoints; reaches external LLM APIs via NAT Gateway (step 14).
- **NAT Gateway + Internet Gateway**: Provide outbound internet access from the private subnet EC2 instance (steps 14 → 15 → 16), used exclusively for external LLM API calls through LiteLLM. Inbound traffic is always routed through the ALB.
- **AWS S3 — Audio Bucket**: Receives audio file uploads via presigned URLs (step 8). An S3 event notification triggers the transcription Lambda on object creation (step 9).
- **AWS Lambda (audio2text_lambda.py)**: Triggered by the S3 Audio bucket on object creation (step 9). Starts an AWS Transcribe job (step 10).
- **AWS Transcribe**: Performs speech-to-text conversion on the audio file and writes the completed transcript to the S3 Transcript bucket (step 11).
- **AWS S3 — Transcript Bucket**: Stores completed transcripts. Read by the EC2 Celery worker during vectorisation (step 12).
- **AWS S3 — Vectors Bucket**: Stores the custom dual-granularity vector index (chunk embeddings + segment summaries + global summary). Read and written by the EC2 instance (step 13).
- **Amazon DynamoDB**: Stores per-recording processing status, progress percentage, stage label, short AI summary, transcript ID, and per-session conversation memory. Written by both the Lambda user trigger (step 3) and the EC2 Celery worker (step 7).
- **AWS IAM**: Role-based policies scope each service's access — Lambda roles for S3/DynamoDB/Transcribe; EC2 instance profile for S3/DynamoDB; Amplify service role for hosting.

#### Component Design

- **Frontend (AWS Amplify)**: React + Vite SPA with pages for Landing, Login, Register, Confirm, Dashboard, Library, and the AI Assistant chat. Authentication state is managed via AWS Cognito; protected routes redirect unauthenticated users. The `AssistantPage` and `LibraryPage` are the primary user-facing surfaces.
- **API Layer (EC2 — FastAPI)**: FastAPI with two routers — `recordings` (upload URL generation, processing trigger, Q&A, status polling) and `library` (listing and filtering recordings). All endpoints validate Cognito JWT tokens. Co-located on EC2 alongside Celery and Redis.
- **Async Pipeline (EC2 — Celery Worker)**: A Celery worker (`process_audio_task`) orchestrates the full pipeline: it polls the S3 Audio bucket until the upload is confirmed (`wait_until_uploaded`), waits for the Transcribe job to complete, runs vectorisation, and writes status transitions (`Pending → Processing → Completed / Failed`) to DynamoDB at each stage with a numeric progress percentage and stage label. Redis (also on EC2) serves as the Celery broker.
- **Vector & Semantic Index (EC2 → S3 Vectors)**: After transcription, the transcript is split into overlapping chunks using LangChain's text splitter, grouped into topic segments via a sliding-window coherence algorithm, and each segment is summarised by the LLM. Embeddings are generated using Sentence Transformers (multilingual-capable) and persisted to the S3 Vectors bucket at two granularities: individual chunks (precise retrieval) and segment-level summaries (topic-level queries). A global summary is pre-computed and stored for full-meeting summary queries.
- **RAG Question Router**: Each user query is classified by an LLM call with a structured output schema into one of five intents (Factual, Comprehensive, Topic Summary, Full Summary, Off-topic). The router selects the matching retrieval strategy. Confidence scores below a threshold trigger automatic upgrades to a more thorough strategy. LLM calls for classification and generation exit the VPC via NAT Gateway.
- **Conversation Memory (DynamoDB)**: Each user-recording session maintains a memory object in DynamoDB containing a rolling working window (last 10 turns), a compressed summary of older turns, and a source history log. Sessions survive EC2 restarts and can be resumed across devices.

---

### 4. Technical Implementation

#### Implementation Phases

The project follows a six-phase roadmap delivering a production-ready system in twelve weeks:

- **Phase 1 — Infrastructure & CI/CD (Weeks 1–2)**: AWS environment setup (S3, DynamoDB, Cognito, Transcribe), Celery/Redis configuration, deployment pipeline
- **Phase 2 — Core Audio Pipeline (Weeks 3–4)**: Upload flow with presigned URLs, Lambda transcription trigger, status tracking in DynamoDB, Celery task integration
- **Phase 3 — Vector & RAG Engine (Weeks 5–6)**: Text chunking, Sentence Transformer embedding, vector store construction, question classifier, retrieval strategies
- **Phase 4 — API & Chat Interface (Weeks 7–8)**: FastAPI routers, conversation memory management, frontend chat page, session persistence
- **Phase 5 — Dashboard & Library (Weeks 9–10)**: Recording library with status cards, search/filter, meeting history view
- **Phase 6 — QA, Hardening & Launch (Weeks 11–12)**: Load testing, error handling, cost optimisation, documentation, production release

#### Technical Requirements

- **Backend**: Python 3.x, FastAPI 0.135, Celery 5.6, Redis (broker), boto3 1.42, LiteLLM 1.82, Sentence Transformers 5.3, LangChain Text Splitters 1.1, NumPy 2.4, Pydantic 2.12, python-dotenv
- **Frontend**: React + Vite, AWS Amplify/Cognito JS SDK, Axios-based API client
- **Infrastructure**: VPC (ap-southeast-1) with public subnet (ALB) and private subnet (EC2, Gateway Endpoints); EC2 instance running FastAPI + Celery + Redis; NAT Gateway + Internet Gateway for outbound access; three S3 buckets (Audio, Transcript, Vectors); DynamoDB (recording status + user tables); Cognito User Pool; Lambda (two functions); AWS Transcribe; Amplify (frontend hosting); IAM roles
- **LLM**: LiteLLM abstraction layer; default target model is Claude Sonnet; switchable via configuration
- **Audio Formats Supported**: WAV, MP3, M4A
- **Key Design Pattern**: Presigned S3 upload URLs bypass API gateway file-size limits (typically 10 MB), reduce backend egress costs, and improve upload throughput for large audio files

---

### 5. Timeline & Milestones

#### Project Timeline

| # | Timeline | Phase | Deliverables |
|---|---|---|---|
| 1 | Weeks 1–2 | Infrastructure & CI/CD | AWS setup, Celery/Redis, deployment pipeline |
| 2 | Weeks 3–4 | Core Audio Pipeline | Upload flow, Lambda trigger, status tracking, Celery task |
| 3 | Weeks 5–6 | Vector & RAG Engine | Chunking, embedding, vector store, question classifier, retrieval strategies |
| 4 | Weeks 7–8 | API & Chat Interface | FastAPI routers, memory management, frontend chat, session persistence |
| 5 | Weeks 9–10 | Dashboard & Library | Recording library, status cards, search/filter, history |
| 6 | Weeks 11–12 | QA, Hardening & Launch | Load testing, error handling, cost optimisation, documentation, production release |

**Total estimated duration**: 12 weeks

---

### 6. Budget Estimation

#### Infrastructure Costs (AWS Services — ap-southeast-1, Singapore)

The architecture includes both fixed monthly charges (EC2, ALB, NAT Gateway) and variable charges (Transcribe, S3, data transfer). Unlike a purely serverless design, the VPC networking layer carries a meaningful baseline cost regardless of usage volume.

| Service | Est. Monthly Cost | Cost Driver |
|---|---|---|
| **NAT Gateway** | ~$43/month | $0.059/hr × 730 hrs fixed + $0.059/GB data processed; dominant cost — routes EC2 outbound LLM API calls |
| **ALB (Application Load Balancer)** | ~$18/month | ~$0.008/hr base + LCU charges; minimum cost at low request volume |
| **AWS EC2 (t3.small, private subnet)** | ~$15/month | FastAPI + Celery + Redis co-located; t3.small ($0.0208/hr × 730 hrs); upgradeable to t3.medium (~$30/month) if needed |
| **AWS Amplify** | ~$0.01–0.05/month | Frontend hosting; negligible at low traffic |
| **AWS S3 (3 buckets)** | ~$0.10–0.30/month | Audio, Transcript, and Vectors buckets; scales with recording volume |
| **AWS Transcribe** | ~$0.02–0.30/month | $0.024/min; highly variable — scales directly with hours of audio processed |
| **AWS Lambda (2 functions)** | ~$0.00/month | Well within free tier (1M requests/month) |
| **Amazon DynamoDB** | ~$0.00–0.05/month | On-demand pricing; low read/write volume at initial scale |
| **Amazon Cognito** | ~$0.00/month | Free tier: first 50,000 MAUs |
| **Data Transfer (inbound/outbound)** | ~$0.05–0.15/month | S3 inbound free; ALB and EC2 outbound egress at $0.09/GB |

**Estimated total: ~$76–$77/month at minimal scale** (dominated by NAT Gateway ~$43 + ALB ~$18 + EC2 ~$15)

> **Cost optimisation note**: The NAT Gateway is the single largest cost driver (~56% of the fixed baseline). If LLM API call volume is low or can be batched, a VPC Interface Endpoint for Bedrock (if using AWS-hosted models via LiteLLM) could eliminate NAT Gateway traffic for LLM calls and meaningfully reduce this cost. Alternatively, moving to an EC2 instance in the public subnet with an Elastic IP removes the NAT Gateway entirely, though at a minor security trade-off.

#### One-Time Development Costs

- AWS account configuration, VPC setup, IAM role creation, and CI/CD pipeline: minimal, using AWS CDK or console
- No proprietary hardware required — fully cloud-native

---

### 7. Risk Assessment

#### Risk Matrix

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| Transcription latency spikes | High (pipeline stalls, poor UX for large files) | Medium | Async pipeline with real-time progress tracking; configurable timeout with retry logic in Celery task (`max_retries=3`, `countdown=60`) |
| LLM API cost overruns | Medium (budget pressure at scale) | Low | Per-query token budgets; cached segment summaries reduce synthesis calls; LiteLLM enables switching to cheaper models via configuration |
| S3 upload failures | Medium (recording lost before processing) | Low | Worker polls S3 via `wait_until_uploaded` before proceeding; status written to DynamoDB immediately; failed status surfaced in UI |
| Vector index corruption | High (Q&A unavailable for recording) | Low | Vector store keyed by recording ID; re-vectorisation can be triggered by re-running the Celery task |
| Auth token expiry mid-session | Low (user logged out unexpectedly) | Medium | JWT validation on every API call; Cognito handles token refresh; frontend redirects gracefully on 401 |

#### Mitigation Strategies

- **Async resilience**: Celery tasks include automatic retry with exponential back-off on transient failures; status transitions are durably written to DynamoDB at every stage
- **Hallucination prevention**: The base LLM system prompt instructs the model to answer only from provided transcript context; the question router's off-topic classifier further prevents irrelevant LLM calls
- **Secret management**: All API keys, bucket names, DynamoDB table names, and AWS region are stored as environment variables loaded via `python-dotenv`; no secrets are committed to source control

#### Contingency Plans

- If AWS Transcribe is unavailable, the Celery task fails gracefully and marks the recording as `failed` in DynamoDB; the UI surfaces the failure with a retry option
- If the LLM provider is unavailable, LiteLLM can be reconfigured to fall back to an alternative provider without application code changes
- CloudFormation or CDK stacks can be used for rapid infrastructure rollback if a deployment introduces regressions

---

### 8. Expected Outcomes

#### Technical Improvements

- Any word, decision, or action item from any recorded meeting is retrievable in seconds via natural language query
- The dual-granularity vector index serves the full query spectrum — from precise factual lookups to broad thematic summaries — efficiently and with low latency
- The RAG question router reduces unnecessary LLM calls (and associated costs) for simple queries while ensuring comprehensive coverage for complex ones
- Real-time status tracking (pending → processing → completed) replaces opaque batch jobs with a transparent, user-visible pipeline

#### Long-term Value

- The platform's modular architecture (API, worker, vector store, LLM layer) allows each component to be independently scaled or replaced as requirements evolve
- LiteLLM's model-agnostic abstraction ensures the platform can adopt newer, cheaper, or more capable LLM backends without architectural changes
- The vector store and conversation memory layer form a reusable foundation for future AI-powered features (e.g., cross-recording search, speaker diarisation, action-item extraction)
- Transcript and metadata stored in DynamoDB and S3 provide a durable, queryable knowledge base that accumulates value over time as the recording library grows