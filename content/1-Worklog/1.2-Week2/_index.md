---
title: "Week 2 Worklog"
date: 2026-01-19
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Continue deepening knowledge of core **AWS** services.
* Clearly understand how to deploy and configure real-world resources on the cloud.
* Learn how services integrate and support each other within a system.
* Improve hands-on skills with popular services such as **EC2**, **S3**, **CloudWatch**, and **RDS**.
* Get initial exposure to monitoring and managing resources on AWS.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Study **EC2 (Elastic Compute Cloud)** <br>&emsp; + Concept of virtual machines on the cloud <br>&emsp; + Instance types (t2, t3, ...) and their use cases <br>&emsp; + AMI (Amazon Machine Image) <br>&emsp; + Security Groups and firewall mechanisms <br> - Practice launching EC2 instances <br>&emsp; + Select appropriate instance type (Free Tier) <br>&emsp; + Configure key pair for SSH <br>&emsp; + Connect to instance via SSH | 01/19/2026 | 01/19/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Review **IAM** knowledge <br>&emsp; + Review Users, Groups, Policies <br>&emsp; + Recheck access control principles <br> - Practice EC2 again <br>&emsp; + Create and manage multiple instances <br>&emsp; + Configure Security Groups for each instance <br> - Combine **IAM with EC2** <br>&emsp; + Use IAM Users to access resources <br>&emsp; + Understand instance-level access control | 01/20/2026 | 01/20/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - Study **S3 (Simple Storage Service)** <br>&emsp; + Concepts of Bucket and Object <br>&emsp; + Storage classes (Standard, IA, Glacier, ...) <br>&emsp; + Bucket naming rules <br> - Practice with S3 <br>&emsp; + Create buckets <br>&emsp; + Upload/download files <br>&emsp; + Configure access permissions (public/private) <br>&emsp; + Explore data management interface | 01/21/2026 | 01/21/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - Study **CloudWatch** <br>&emsp; + Concept of monitoring in cloud <br>&emsp; + Metrics, Logs, Alarms <br>&emsp; + Role of CloudWatch in system operations <br> - Practice <br>&emsp; + Monitor EC2 instance status <br>&emsp; + Observe CPU, Network, Disk metrics <br>&emsp; + Create basic alarms | 01/22/2026 | 01/22/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - Study **RDS (Relational Database Service)** <br>&emsp; + Supported databases (MySQL, PostgreSQL, ...) <br>&emsp; + Concept of managed database <br>&emsp; + Basic configuration (instance class, storage, security) <br> - Practice deploying RDS <br>&emsp; + Create database instance <br>&emsp; + Configure Security Groups for connection <br>&emsp; + Connect to database from local/EC2 <br>&emsp; + Test operation and basic queries | 01/23/2026 | 01/23/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 2 Achievements:

* Gained a deeper understanding of **EC2**:
  * Know how to choose the right instance type for specific needs
  * Understand the process of launching and configuring virtual machines
  * Confident in connecting to instances via SSH

* Reinforced and applied **IAM** knowledge:
  * Applied access control when working with EC2
  * Better understanding of security and access management
  * Avoided using the root account in practical tasks

* Mastered basic operations with **S3**:
  * Create and manage buckets
  * Upload, download, and organize data
  * Understand data access control mechanisms

* Learned how to use **CloudWatch**:
  * Monitor resource status and performance
  * Read and understand basic metrics
  * Set up alerts for system monitoring

* Explored **RDS**:
  * Understand the concept of managed database services
  * Successfully deploy database instances
  * Connect to and test database operations

* Understood service integration:
  * EC2 + IAM → Access management and security
  * EC2 + S3 → Data storage and processing
  * EC2 + CloudWatch → System monitoring
  * EC2 + RDS → Building a complete backend system