---
title : "Networking"
date: "2000-01-01"
weight: 3
chapter : false
pre : " <b> 4.3. </b> "
---

In this section you will build the complete network foundation for the Voice Summarizer platform, then wire your custom domain to the platform's endpoints using Amazon Route 53.

The two sub-sections must be completed in order — the Route 53 alias records point to infrastructure (ALB, Amplify) that is created during the VPC and ALB setup steps.

| Sub-section | What you will create |
|---|---|
| VPC Setup | VPC, public/private subnets, Internet Gateway, NAT Gateway, route tables, Gateway Endpoints (S3 + DynamoDB), Security Group |
| Route 53 DNS | Public Hosted Zone, NS record delegation at your registrar, alias A record pointing to the ALB |

#### Content

- [VPC Setup](4.3.1-vpc/)
- [Route 53 DNS](4.3.2-route53/)
