---
title: "Workshop"
date: "2026-01-11"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# Voice Summarizer — AI-Powered Audio Intelligence Platform

#### Overview

This guide provides a complete, step-by-step procedure for deploying the Voice Summarizer platform on AWS. This system leverages **Amazon Cognito**, **AWS Amplify**, **S3**, **DynamoDB**, **Lambda**, and **AWS Transcribe** for the audio intelligence pipeline, with the backend hosted on **EC2** inside a **VPC** and exposed through an **Application Load Balancer**. Users can upload audio recordings — meetings, lectures, interviews, and calls — which are automatically transcribed, semantically indexed, and made queryable through a conversational AI interface powered by **LiteLLM**.

#### Content

1. [Workshop Overview](4.1-Workshop-overview/)
2. [Prerequisites](4.2-Prerequisites/)
3. [VPC & Networking](4.3-vpc/)
4. [Storage Setup](4.4-storage/)
5. [Lambda Functions](4.5-lambda/)
6. [EC2 Setup](4.6-ec2/)
7. [Application Load Balancer](4.7-alb/)
8. [Cognito User Pool](4.8-cognito/)
9. [Amplify Deployment](4.9-amplify/)
10. [Clean Up](4.10-cleanup/)
11. [Appendix](4.11-Appendices/)