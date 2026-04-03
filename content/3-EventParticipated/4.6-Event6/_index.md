---
title: "Event 6"
date: "2025-11-29"
weight: 06
chapter: false
pre: " <b> 4.6. </b> "
---

# Summary Report: “AWS Cloud Mastery Series #3: AWS Well-Architected – Security Pillar Workshop”

### Event Objectives

- AWS Cloud Club Introduction
- Pillar 1: Identity a& Access Management (IAM)
- Pillar 2: Detection & Continuous Monitoring
- Pillar 3: Infrastructure Protection
- Pillar 4: Data Protection
- Pillar 5: Incident Response

### Speakers

- **Le Vu Xuan An** - AWS Cloud Club Captain HCMUTE
- **Tran Duc Anh** - AWS Cloud Club Captain SGU
- **Tran Doan Cong Ly** - AWS Cloud Club Captain PTIT
- **Danh Hoang Hieu Nghi** - AWS Cloud Club Captain HUFLIT

- **Huynh Hoang Long** - AWS Community Builders
- **Dinh Le Hoang Anh** - AWS Community Builders

- **Nguyen Tuan Thinh** - Cloud Engineer Trainee
- **Nguyen Do Thanh Dat** - Cloud Engineer Trainee

- **Van Hoang Kha** - Cloud Security Engineer, AWS Community Builder

- **Thinh Lam** - FCJ Member
- **Viet Nguyen** - FCJ Member

- **Mendel Grabski (Long)** - Ex-Head of Security & DevOps, Cloud Security Solution Architect
- **Tinh Truong** - Platform Engineer at TymeX, AWS Community Builder 

### Key Highlights

### AWS Cloud Club
An introduction to AWS Cloud Club: 
- Helps explore and grow cloud computing skills
- Develop technical leadership
- Build meaningful connections globally
Provides hands-on AWS Experiences, mentorship with AWS Professional and long-term career support

AWS Cloud Clubs that are part of FCJA:
- AWS Cloud Club HCMUTE
- AWS Cloud Club SGU
- AWS Cloud Club PTIT
- AWS Cloud Club HUFLIT

Benefits: Build Skills, Community and Opportunities

### Identity & Access Management (IAM)
IAM is an essential AWS service, responsible for controlling secure access. IAM manages Users, Groups, Roles, and Permissions, and ensures both authentication and authorization.

- Best Practices include:

    + Least Privilege Principle.

    + Delete root access keys post-creation.

    + Avoid "*" in Actions/Resources

    + Use Single Sign-On (SSO) for multi-account integration and centralized access management.

- Service Control Policies (SCPs): Organization-level policies that set the maximum available permissions for all accounts within an Organization. SCPs only filter permissions; they never grant permissions.

- Permission Boundaries: Sets the maximum permissions that an identity-based policy can grant to a specific User/Role within an account.

- MFA :
| TOTP (Time-based One-Time Password) | FIDO2 (Fast Identity Online 2) |
| :--- | :--- |
| **Shared secret** | **Public-key cryptography** |
| Requires manually typing a 6-digit code | Requires a simple touch or biometric scan |
| Free | Variable |
| Flexible backups and recovery | Strict backups and zero recovery

- Credential Rotation with AWS Secrets Manager:
    + Credential Updater uses Secrets Manager functions in a cycle: Create Secret, Set Secret (e.g., every 7 days) Test Secret, and Finish Secret
    + Rotation events can be sent to an EventBridge Schedule for timing control.
    + Finally deprecate previous Secret

### Detection and Continous Monitoring
- Multi-Layer Security Visibility:
    + Management Events:** API calls and console actions across all organization accounts.
    + Data Events:** S3 object access and Lambda executions at scale.
    + Network Activity Events:** VPC Flow Logs integration for network-level monitoring.
    + Organization Coverage:** Unified logging across all member accounts and regions.

- Alerting & Automation with EventBridge

    + Real-time Events: CloudTrail events flow to EventBridge for immediate processing. This is the foundation of Event-Driven Architecture (EDA), allowing systems to react to changes as they occur.

    + Automated Alerting: Detect suspicious activities across all organization accounts.

    + Cross-account Event Routing: Centralized event processing and automated response. EventBridge makes this seamless, routing events based on rules to targets across accounts or regions.

    + Integration & Workflows: Lambda, SNS, SQS integration for automated security workflows.

- Detection-as-Code:
    
    + CloudTrail Lake Queries: Creating and using SQL-based detection rules for advanced threat hunting.

    + Version-Controlled Logic: Detection rules and logic are tracked and managed through code repositories. 

    + Automated Deployment: The trails and detection rules are deployed automatically across all relevant organization accounts, ensuring uniform security coverage.

    + Infrastructure-as-Code (IaC): Uses IaC tools for the automated setup and configuration of the organization's logging and event trails
### Guard Duty
- GuardDuty is an Always-On, Intelligent Threat Detection solution

- How GuardDuty Works – Rely on continuous analysis of **Three Pillars of Detection**:

| Data Source | What It Monitors | Real-World Example |
| :--- | :--- | :--- |
| **CloudTrail Events** | IAM actions, permission changes, API calls | Attacker disables logging to cover tracks. |
| **VPC Flow Logs** | Network traffic to/from your resources | EC2 sending data to a botnet C2 server. |
| **DNS Logs** | DNS queries from your infrastructure | Malware-infected queries cryptomining sites. |

- Advanced Protection Plans: GuardDuty offers specialized detection add-ons for complete coverage:

    + S3 Protection: Detects abnormal S3 access patterns and scans malware in S3 objects at upload time.

    + EKS Protection: Monitors Kubernetes audit logs for unauthorized access and chains findings with S3 to map the full attack path.

    + Malware Protection: Automatically scans EBS volumes of EC2 instances when compromise is suspected.

    + RDS Protection: Analyzes login activity logs to databases (Aurora/RDS) and detects brute-force attacks (multiple failed login attempts from a single IP).

    + Lambda Protection: Monitors network logs flowing from Lambda function invocations and detects if a compromised function sends data to malicious IPs.

    + Runtime Monitoring – Deep Inside Your OS: Achieved using a GuardDuty Agent installed on EC2/EKS/ECS Fargate. It monitors running processes, file access patterns, system calls, and attempts at privilege escalation or reverse shells.

- Compliance Standards:

    + AWS Foundational Security Best Practices: Developed by AWS and covers a range of AWS services.

    + CIS AWS Foundations Benchmark: developed by: AWS and industry professionals focusing on Identity (IAM), Logging & Monitoring, and Networking.

- Compliance Enforcement with Detection-as-Code

    + IaC Tool: AWS CloudFormation is used to deploy configurations.

    + Compliance Engine: AWS CloudFormation pushes configuration checks to AWS Security Hub CSPM.

    + Compliance Standards Applied: Security Hub performs checks against the listed standards (AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark, PCI DSS, NIST).

    + Resources Covered: Amazon S3, Amazon EC2, and Amazon RDS.

### Network Security Controls

- Attack Vectors: Threats are categorized into Ingress Attacks (DDoS, SQL injection), Egress Attacks (data exfiltration, DNS tunneling), and Inside Attacks (lateral movement).

- Security Groups (SG): Act as a Stateful firewall at the instance/interface level. They only support allow rules and have an implicit deny all.

- Network ACLs (NACLs): Operate at the subnet level. They are stateless and use numbered rules to explicitly ALLOW or DENY traffic.

- AWS TGW Security Group Referencing: Allows Transit Gateway (TGW) VPCs to define Inbound rules using only SG references.

- Route 53 Resolver: Routes DNS queries to Private DNS (private hosted zones), VPC DNS, or Public DNS.

- AWS Network Firewall:

    + Use Cases: Egress filtering (blocking bad domains, malicious protocols), Environment segmentation (VPC to VPC), and Intrusion prevention (IDS/IPS rules).

    + Active Defense: Can automatically block malicious traffic using Amazon Threat Intelligence, where GuardDuty findings are marked for automated blocking.

### Data Protection & Governance

- Encryption (KMS): Data is encrypted using a Data Key, which is protected by a Master Key (CMK). KMS policies enforce the second secure layer with Condition keys to define When encryption/decryption is allowed.

- Certificate Management (ACM): Provides free public certificates and automatically renews certificates 60 days before expiration. DNS Validation is the recommended validation method.

- Secrets Manager: Solves the problem of hardcoded credentials. It uses a 4-step Lambda logic (createSecret, setSecret, testSecret, finishSecret) for automatic credential rotation without downtime.

- API Service Security (S3 & DynamoDB): S3 requires TLS 1.2+ and bucket policies with aws:SecureTransport for enforcement. DynamoDB is secure by default with mandatory HTTPS.

- Database Service Security (RDS): Requires client-side trust in the AWS Root CA Bundle to verify server identity, and server-side enforcement (e.g., rds.force_ssl=1 for PostgreSQL).

### Incident Response & Prevention

- Prevention Best Practices: Key steps include using temporary credentials, never exposing S3 buckets directly, placing sensitive services behind private subnets, managing everything through Infrastructure as Code, and using double-gate high-risk changes (PR approval, pipeline deployment).

- Incident Response Process: A structured 5-step approach: Preparation, Detection & Analysis, Containment (isolate, revoke credentials), Eradication & Recovery, and Post-Incident (lessons learned).

### Event Experience

- The event is extremely useful for our team, aligns directly with our project of Automated Incident Response and Forensics

- Q: Our team's project is an Automated Incident Response and Forensics tool with Guard Duty being the main focus of our incident response, but from our testing we can see that Guard Duty can take up to 5 minutes to generate a finding when an incident occured, we want to ask are there any solutions to reduce this latency?

- A: Guard Duty takes 5 minutes to generate findings is just something you have to accept, as its the way Guard Duty is configured to work: Guard Duty have to go through a large amount of security data set to determine the exact threat and then generate a finding. If you want to reduce latency however, one of the ways you solve it is with 3rd party security services integration such as: Open Clarity Free for almost realtime findings and also you can detech anomalies and unsual user behavior with CloudTrail

- Mr.Mendel Grabski was very keen to offer his support when we asked about our project after the event

#### Some event photos

![All Attendee Picture](/images/4-Event/Event6AttendeePic.jpg)
_Picture of all Attendee_

![Group Picture With Speaker Mendel Grabski and Speaker Van Hoang Kha](/images/4-Event/Event6PicturewithSpeakers.jpg)
_Group Picture With Speaker Mendel Grabski and Speaker Van Hoang Kha_