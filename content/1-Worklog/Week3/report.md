---
title: "Week 3 Worklog"
date: 2026-01-26
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Expand knowledge of advanced **AWS** services.
* Understand how to build scalable systems and optimize performance.
* Get familiar with services related to content delivery and routing.
* Get introduced to automation tools such as **AWS CLI**.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Study **Route 53** <br>&emsp; + Concept of **DNS** and domain name resolution <br>&emsp; + Hosted Zones (Public/Private) <br>&emsp; + Record types (A, CNAME, NS, ...) <br>&emsp; + Routing policies (Simple, Weighted, Latency, ...) <br> - Practice <br>&emsp; + Create Hosted Zones <br>&emsp; + Configure domain routing to resources (EC2/S3) <br>&emsp; + Test domain resolution | 01/26/2026 | 01/26/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Review **EC2** and foundational knowledge <br> - Study **Auto Scaling** <br>&emsp; + Scaling concepts (vertical vs horizontal) <br>&emsp; + Launch Template/Configuration <br>&emsp; + Auto Scaling Group (ASG) <br> - Practice Auto Scaling deployment <br>&emsp; + Create Launch Template <br>&emsp; + Create Auto Scaling Group <br>&emsp; + Configure scaling policy based on CPU <br>&emsp; + Test automatic scaling behavior | 01/27/2026 | 01/27/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - Get familiar with **AWS CLI** <br>&emsp; + Install AWS CLI on local machine <br>&emsp; + Configure credentials (Access Key, Secret Key) <br>&emsp; + Understand CLI command structure <br> - Practice <br>&emsp; + Create, list, delete resources (EC2, S3) via CLI <br>&emsp; + Compare CLI vs Console operations <br>&emsp; + Understand benefits of automation and scripting | 01/28/2026 | 01/28/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - Study **DynamoDB** <br>&emsp; + Concept of NoSQL database <br>&emsp; + Table, Item, Attribute <br>&emsp; + Primary Key (Partition key, Sort key) <br>&emsp; + Advantages in performance and scalability <br> - Practice <br>&emsp; + Create DynamoDB table <br>&emsp; + Insert, update, delete data <br>&emsp; + Perform basic queries | 01/29/2026 | 01/29/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - Study **CloudFront** <br>&emsp; + Concept of **CDN (Content Delivery Network)** <br>&emsp; + Edge locations and caching <br>&emsp; + Distribution types (Web/RTMP) <br> - Practice <br>&emsp; + Create CloudFront distribution <br>&emsp; + Connect S3/EC2 as origin <br>&emsp; + Test access speed and caching <br>&emsp; + Compare performance with and without CDN | 01/30/2026 | 01/30/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 3 Achievements:

* Gained a solid understanding of **Route 53**:
  * Understand how DNS works
  * Know how to configure domains and basic record types
  * Understand traffic routing mechanisms

* Successfully applied **Auto Scaling**:
  * Deployed Auto Scaling Groups for EC2
  * Understand how systems automatically scale based on load
  * Recognize the importance of scaling in real-world systems

* Got familiar with **AWS CLI**:
  * Managed resources using command line
  * Better understanding of automation and scripting
  * Compared advantages and disadvantages of CLI vs Console

* Explored **DynamoDB**:
  * Understand NoSQL database model
  * Practiced creating tables and managing data
  * Recognized differences between NoSQL and relational databases

* Learned principles of **CloudFront**:
  * Understand how CDN reduces latency and improves performance
  * Deployed content distribution via edge locations
  * Observed benefits of caching in web systems

* Improved understanding of AWS architecture:
  * Route 53 → Domain routing
  * CloudFront → Content acceleration
  * EC2 + Auto Scaling → Flexible load handling
  * DynamoDB → High-performance data storage