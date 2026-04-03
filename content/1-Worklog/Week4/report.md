---
title: "Week 4 Worklog"
date: 2026-02-02
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Master networking components in **AWS** and learn how to design basic network architecture.
* Clearly understand network-level security mechanisms and how to control resource access.
* Get familiar with services that improve system scalability and performance.
* Reinforce knowledge through hands-on labs.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Study **VPC (Virtual Private Cloud)** <br>&emsp; + Concept of private network in the cloud <br>&emsp; + CIDR blocks and subnetting <br>&emsp; + Public vs Private Subnets <br>&emsp; + Route Tables and routing <br>&emsp; + Internet Gateway and NAT Gateway <br> - Practice <br>&emsp; + Create a new VPC <br>&emsp; + Create and configure subnets <br>&emsp; + Set up route tables for public/private subnets <br>&emsp; + Enable Internet access for EC2 instances | 02/02/2026 | 02/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Study network security mechanisms <br>&emsp; + **Security Groups** (stateful firewall) <br>&emsp; + **Network ACLs** (stateless firewall) <br>&emsp; + Compare Security Groups vs NACLs <br> - Practice <br>&emsp; + Create allow/deny traffic rules <br>&emsp; + Test SSH/HTTP access <br>&emsp; + Apply rules to restrict IP access | 02/03/2026 | 02/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - Perform integrated labs <br>&emsp; + Deploy EC2 within the created VPC <br>&emsp; + Connect EC2 to the Internet and test access <br>&emsp; + Integrate S3 with EC2 for storage <br> - Troubleshoot issues related to networking and access configuration | 02/04/2026 | 02/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - Study **Elastic Load Balancing (ELB)** <br>&emsp; + Concept of load balancing <br>&emsp; + Application Load Balancer (ALB) <br>&emsp; + Network Load Balancer (NLB) <br>&emsp; + Listener and Target Groups <br> - Practice <br>&emsp; + Create a Load Balancer <br>&emsp; + Connect it to EC2 instances <br>&emsp; + Test traffic distribution <br> - Combine with **Auto Scaling Group** for better scalability | 02/05/2026 | 02/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - Explore **ElastiCache** <br>&emsp; + Concept of caching and its role in systems <br>&emsp; + Redis and Memcached <br>&emsp; + Real-world use cases of caching <br> - Practice <br>&emsp; + Create cache instance (Redis) <br>&emsp; + Connect and test basic functionality <br>&emsp; + Understand how caching reduces database load | 02/06/2026 | 02/06/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 4 Achievements:

* Gained a solid understanding of networking in **AWS**:
  * Understand the structure of a complete **VPC**
  * Differentiate and deploy Public/Private Subnets
  * Configure routing using Route Tables
  * Enable Internet connectivity via Internet Gateway and NAT Gateway

* Mastered security mechanisms:
  * Clearly distinguish between **Security Groups** and **Network ACLs**
  * Understand stateful vs stateless behavior
  * Configure rules to properly control resource access

* Reinforced knowledge through labs:
  * Deployed EC2 systems within a real VPC environment
  * Troubleshot network and permission issues
  * Gained a clear understanding of traffic flow in the system

* Understood load balancing principles:
  * Use **Load Balancer** to distribute traffic
  * Combine with **Auto Scaling** for better scalability
  * Recognize the importance of load balancing in production systems

* Explored **ElastiCache**:
  * Understand the role of caching in improving performance
  * Know how to reduce backend database load
  * Practiced basic cache deployment with Redis

* Improved understanding of system architecture:
  * VPC → Network foundation
  * Security Groups/NACLs → Security layer
  * ELB + Auto Scaling → High availability
  * ElastiCache → Performance optimization