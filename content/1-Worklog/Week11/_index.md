---
title: "Week 11 Worklog"
date: 2026-03-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Continue developing the project into a **fullstack application** on AWS.
* Deploy real infrastructure using EC2 and proper network configuration.
* Focus on building the **backend service** and start developing the **frontend**.
* Integrate system components to form a more complete application.
* Get familiar with real-world cloud application development workflows.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Deploy **EC2 instance** <br>&emsp; + Choose appropriate instance (t2.micro / free tier) <br>&emsp; + Configure security groups (SSH, HTTP) <br> - Set up **VPC and Subnets** <br>&emsp; + Public subnet for EC2 <br>&emsp; + Configure Internet Gateway routing <br> - Prepare environment <br>&emsp; + Install Python, pip, virtual environment <br>&emsp; + Set up application runtime environment | 03/23/2026 | 03/23/2026 | |
| Tue | - Start developing **backend with Python** <br>&emsp; + Build project structure (MVC or modular) <br>&emsp; + Create basic RESTful APIs <br> - Develop logic modules <br>&emsp; + Handle request/response <br>&emsp; + Connect to database <br> - Test APIs using Postman | 03/24/2026 | 03/24/2026 | |
| Wed | - Complete backend <br>&emsp; + Build **basic chat functionality** <br>&emsp; + Handle sending/receiving messages <br> - Test integration between components <br>&emsp; + API ↔ Database <br>&emsp; + Backend ↔ Client | 03/25/2026 | 03/25/2026 | |
| Thu | - Extend chat system <br>&emsp; + Store chat history <br>&emsp; + Design data schema <br>&emsp; + Optimize query performance <br> - Test and optimize backend <br>&emsp; + Error handling <br>&emsp; + Basic logging | 03/26/2026 | 03/26/2026 | |
| Fri | - Study **Amazon Cognito** <br>&emsp; + Authentication (Sign up / Login) <br>&emsp; + User Pools <br> - Explore **AWS Amplify** <br>&emsp; + Frontend deployment <br>&emsp; + Simple hosting <br> - Connect frontend with backend <br>&emsp; + Call APIs <br>&emsp; + Test end-to-end flow | 03/27/2026 | 03/27/2026 | |

---

### Week 11 Achievements:

* Successfully deployed infrastructure:
  * Set up **EC2, VPC, Subnets, Security Groups**

* Built a complete backend:
  * Developed APIs using Python
  * Implemented business logic
  * Connected to database for data storage

* Implemented a basic chat system:
  * Enabled message exchange between client and server
  * Ensured stable data flow

* Extended system features:
  * Stored chat history
  * Optimized queries and data structure
  * Improved backend performance

* Started frontend development:
  * Used **Cognito** for user management
  * Deployed UI with **Amplify**
  * Connected frontend to backend via APIs

* Completed an end-to-end system:
  * Frontend → API → Backend → Database
  * Users can interact with a real working system