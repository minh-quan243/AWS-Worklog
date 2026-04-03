---
title: "Week 5 Worklog"
date: 2026-02-09
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Get introduced to the **serverless** model in AWS and understand how serverless architecture works.
* Learn how to build APIs and connect services in distributed systems.
* Get familiar with system management tools and secure resource access.
* Develop a mindset for designing flexible, scalable, and cost-efficient systems.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Study **AWS Lambda** <br>&emsp; + Concept of **serverless computing** <br>&emsp; + How Lambda functions work (event-driven) <br>&emsp; + Runtime, handler, trigger <br>&emsp; + Basic limits and pricing <br> - Practice <br>&emsp; + Create Lambda functions using Node.js/Python <br>&emsp; + Implement simple logic (return JSON) <br>&emsp; + Test functions directly in the console | 02/09/2026 | 02/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Study **API Gateway** <br>&emsp; + Concept of REST API <br>&emsp; + Endpoints, Methods (GET, POST, ...) <br>&emsp; + Integration with Lambda <br> - Practice <br>&emsp; + Create REST API <br>&emsp; + Connect API to Lambda function <br>&emsp; + Deploy API and test via Postman/browser | 02/10/2026 | 02/10/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - Perform integrated lab **Lambda + API Gateway** <br>&emsp; + Build API to handle requests (e.g., return JSON data) <br>&emsp; + Handle input/output between client and Lambda <br>&emsp; + Debug integration issues <br> - Understand workflow: Client → API Gateway → Lambda → Response | 02/11/2026 | 02/11/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - Study **AWS Systems Manager (Session Manager)** <br>&emsp; + Concept of managing instances without SSH <br>&emsp; + Security benefits (no need to open port 22) <br>&emsp; + IAM roles for EC2 <br> - Practice <br>&emsp; + Configure Session Manager <br>&emsp; + Access EC2 via browser/CLI <br>&emsp; + Compare with traditional SSH access | 02/12/2026 | 02/12/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - Explore **AWS CloudFormation** <br>&emsp; + Concept of **Infrastructure as Code (IaC)** <br>&emsp; + Templates (YAML/JSON) <br>&emsp; + Stack and lifecycle <br> - Practice <br>&emsp; + Write simple templates (create S3/EC2) <br>&emsp; + Deploy stacks from templates <br>&emsp; + Update and delete stacks <br>&emsp; + Observe automated provisioning process | 02/13/2026 | 02/13/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 5 Achievements:

* Gained a clear understanding of the **serverless** model:
  * Understand how **AWS Lambda** works
  * Grasp the event-driven model
  * Able to deploy backend logic using Lambda functions

* Built basic APIs:
  * Use **API Gateway** to create endpoints
  * Integrate APIs with Lambda for request handling
  * Understand request/response flow in serverless systems

* Completed hands-on labs:
  * Deployed a complete API (Client → API → Lambda)
  * Debugged and resolved integration issues
  * Improved log reading and troubleshooting skills

* Got familiar with **Systems Manager**:
  * Access EC2 using **Session Manager**
  * Reduce dependency on SSH and key pairs
  * Improve system security

* Explored **CloudFormation (IaC)**:
  * Understand benefits of infrastructure as code
  * Write and deploy basic templates
  * Recognize automation and reusability advantages

* Improved understanding of modern architecture:
  * API Gateway + Lambda → Serverless backend
  * IAM + Session Manager → Secure access
  * CloudFormation → Automated infrastructure