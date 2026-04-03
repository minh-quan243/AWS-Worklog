---
title: "Week 8 Worklog"
date: 2026-03-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Explore **advanced security services in AWS**.
* Understand how to protect systems, data, and applications in the cloud.
* Get familiar with tools for access control, encryption, and security monitoring.

---

### Tasks Completed During the Week:

| Day | Tasks | Start Date | End Date | Resources |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| Mon | - Study **AWS Security Hub** <br>&emsp; + Overview of security compliance <br>&emsp; + Integration with other security services <br>&emsp; + Security standards (CIS, AWS Foundational Security Best Practices) <br> - Explore **VPC Endpoint (S3 Gateway Endpoint)** <br>&emsp; + Private access to S3 without Internet <br> - Practice <br>&emsp; + Enable Security Hub <br>&emsp; + Configure VPC Endpoint for S3 <br>&emsp; + Test private access | 03/02/2026 | 03/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Study **AWS WAF (Web Application Firewall)** <br>&emsp; + Concept of web application protection <br>&emsp; + Web ACL and Rules <br>&emsp; + Rule types (IP match, rate limit, managed rules) <br> - Practice <br>&emsp; + Create Web ACL <br>&emsp; + Apply rules to block IPs or abnormal requests <br>&emsp; + Integrate with CloudFront/ALB | 03/03/2026 | 03/03/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - Study **AWS KMS (Key Management Service)** <br>&emsp; + Encryption concepts (at rest, in transit) <br>&emsp; + Customer Managed Key vs AWS Managed Key <br>&emsp; + Key rotation <br> - Practice <br>&emsp; + Create KMS key <br>&emsp; + Apply encryption to S3 or EBS <br>&emsp; + Verify key access permissions | 03/04/2026 | 03/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - Explore **AWS Secrets Manager** <br>&emsp; + Manage credentials (DB password, API keys) <br>&emsp; + Automatic rotation <br> - Study **AWS Firewall Manager** <br>&emsp; + Centralized security policy management <br> - Practice <br>&emsp; + Store secrets <br>&emsp; + Retrieve secrets from applications <br>&emsp; + Configure basic policies | 03/05/2026 | 03/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - Study **Amazon Cognito** <br>&emsp; + Authentication vs Authorization <br>&emsp; + User Pools and Identity Pools <br>&emsp; + JWT tokens <br> - Practice <br>&emsp; + Create User Pool <br>&emsp; + Register and log in users <br>&emsp; + Test basic authentication for applications | 03/06/2026 | 03/06/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 8 Achievements:

* Gained a strong understanding of overall security tools:
  * Use **Security Hub** to monitor and assess security posture
  * Apply security standards and detect system vulnerabilities

* Learned how to configure private access:
  * Use **VPC Endpoint** to access S3 internally
  * Reduce risks of exposing resources to the Internet

* Got familiar with application protection:
  * Configure **AWS WAF** to filter requests
  * Understand how to mitigate common attacks (DDoS, injection, brute force)

* Mastered data encryption mechanisms:
  * Use **KMS** to manage encryption keys
  * Apply encryption to stored data
  * Understand the role of encryption in system security

* Managed sensitive information:
  * Use **Secrets Manager** to store credentials
  * Apply best practice of not hardcoding secrets
  * Understand how rotation improves security

* Explored user authentication:
  * Use **Cognito** for user management
  * Understand authentication and authorization processes
  * Get familiar with token-based authentication

* Improved understanding of AWS security architecture:
  * IAM → Identity control
  * WAF → Application protection
  * KMS → Encryption
  * Secrets Manager → Secret management
  * Cognito → User authentication