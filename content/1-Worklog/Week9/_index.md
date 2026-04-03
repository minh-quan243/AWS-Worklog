---
title: "Week 9 Worklog"
date: 2026-03-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Explore **distributed systems** and **event-driven architecture** on AWS.
* Understand how to build **CI/CD pipelines** for automated development and deployment.
* Get familiar with **containerization** and the role of containers in cloud systems.
* Improve skills in designing workflows and orchestrating services.
* Prepare the foundation for building production-ready systems.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Study messaging systems in AWS <br>&emsp; + **Amazon SQS (Simple Queue Service)**: asynchronous queue processing <br>&emsp; + **Amazon SNS (Simple Notification Service)**: publish/subscribe model <br>&emsp; + Compare SQS and SNS <br> - Practice <br>&emsp; + Create queues and topics <br>&emsp; + Send/receive messages between services <br>&emsp; + Simulate asynchronous workflows between producer and consumer | 03/09/2026 | 03/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Get familiar with **Docker** <br>&emsp; + Concept of containerization <br>&emsp; + Images and Containers <br>&emsp; + Dockerfile and build process <br> - Practice <br>&emsp; + Write a simple Dockerfile <br>&emsp; + Build image <br>&emsp; + Run container locally and test application | 03/10/2026 | 03/10/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - Study **CI/CD with AWS CodePipeline** <br>&emsp; + Pipeline workflow (Source → Build → Deploy) <br>&emsp; + Integration with GitHub/CodeCommit <br> - Practice <br>&emsp; + Create a simple pipeline <br>&emsp; + Automatically build and deploy when code changes <br>&emsp; + Monitor pipeline execution | 03/11/2026 | 03/11/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - Study **AWS Storage Gateway** <br>&emsp; + Hybrid storage (on-premise to cloud integration) <br>&emsp; + Real-world use cases <br> - Explore advanced **DynamoDB** <br>&emsp; + Table design based on access patterns <br>&emsp; + Partition key and performance optimization <br> - Practice <br>&emsp; + Create table and test queries | 03/12/2026 | 03/12/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - Study **AWS Step Functions** <br>&emsp; + Workflow orchestration <br>&emsp; + State machine (Task, Choice, Parallel) <br> - Practice <br>&emsp; + Build simple workflow <br>&emsp; + Integrate with Lambda <br>&emsp; + Monitor execution flow | 03/13/2026 | 03/13/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 9 Achievements:

* Gained a strong understanding of **messaging and event-driven architecture**:
  * Use **SQS** for asynchronous queue processing
  * Apply **SNS** for publish/subscribe patterns
  * Reduce coupling between services

* Got familiar with **Docker**:
  * Build images and run containers
  * Understand the role of containers in application packaging
  * Recognize benefits for multi-environment deployment

* Learned **CI/CD processes**:
  * Build pipelines using **CodePipeline**
  * Automate build and deployment processes
  * Reduce manual effort in software development

* Explored hybrid systems and advanced databases:
  * Understand integration between on-premise and cloud using **Storage Gateway**
  * Apply NoSQL design thinking with **DynamoDB**

* Learned **workflow orchestration**:
  * Use **Step Functions** to coordinate services
  * Design workflows using state machines
  * Combine multiple services into a complete workflow

* Strengthened understanding of modern system architecture:
  * Messaging → SQS/SNS
  * Container → Docker
  * Automation → CI/CD Pipeline
  * Orchestration → Step Functions