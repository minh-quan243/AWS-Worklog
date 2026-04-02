---
title: "Event 4"
date: "2025-11-17"
weight: 04
chapter: false
pre: " <b> 4.4. </b> "
---

# Summary Report: “AWS Cloud Mastery Series #2 - DevOps on AWS”

### Event Objectives

- Introduce AWS DevOps Services – CI/CD Pipeline
- Introduce Infrastructure as Code (IaC) and related tools
- Introduce Container Services on AWS
- Ensure Monitoring and Observability capability using AWS Services

### Speakers

- **Truong Quang Tinh** – AWS Community Builder, Platform Engineer - TymeX
- **Bao Huynh** – AWS Community Builder
- **Nguyen Khanh Phuc Thinh** – AWS Community Builder
- **Tran Dai Vi** – AWS Community Builder
- **Huynh Hoang Long** – AWS Community Builder
- **Pham Hoang Quy** – AWS Community Builder
- **Nghiem Le** – AWS Community Builder
- **Dinh Le Hoang Anh** - Cloud Engineer Trainee, First Cloud AI Journey

### Key Highlights

### DevOps Mindset

**- Culture:** Collaboration, Automation, Continuous Learning and Measurement
**- DevOps Roles:** DevOps Engineer, Cloud Engineer, Platform Engineer, Site Reliability Engineer
**- Success Metrics:**
    + Ensure deployment health
    + Improve agility
    + System stability
    + Optimize customer Experience
    + Justify technology investments

| DO | DON'T |
| :--- | :--- |
| Start with Fundamentals | Stay in Tutorial Hell |
| Learn by Building Real Projects | Copy-paste blindly |
| Document Everything | Compare Your Progress to Others |
| Master one thing at a time | Give Up After Failures |
| Soft Skills Enhancement | |

**- Continuous Integration:** Team members integrate their work frequently, aims for continuous Delivery and Deployment

### Infrastructure as Code (IaC)

**-Benefits:** Automation, Scalability, Reproducibility and better Collaboration

#### AWS CloudFormation

AWS's own built in IaC tool, use templates written with YAML or JSON, can build every AWS Infrastructure automatically

**- Stack:** A set of AWS Resources defined in a template, can be used by CloudFormation to create, update or delete said resources

**- CloudFormation Template:** A YAML/JSON file that define an AWS Infrastructure, act like a blueprint to deploy and configure resources

**- How it works:** Create template -> Store in S3 Bucket or Local storage -> Use CloudFormation to create Stacks based on template -> CloudFormation built resources 

**- Drift Detection:** Detect changes in the infrastructure compared to the Stack => Update Stack or revert change, useful for versioning 

#### AWS Cloud Development Kit(CDK)

Open-source software development framework, support IaC using real programming languages(Python,Java,C#.Net, Type/JavaScript and Go)

**- Construct:** Building blocks, comprised of components that represent AWS Resources and their Configuration, have 3 construct level:
  + L1 Construct: Low-level resources maps directly to a single AWS CloudFormation resource
  + L2 Construct: Provide a higher-level abstraction through an intuitive intent-based API,encapsulate best practices and security defaults 
  + L3 Construct: Complete architecture patterns with multiple resources, opinionated implementation and fast deployment

#### AWS Amplify

AWS platform that makes it easy to build, deploy, and scale web and mobile apps, uses CloudFormation under the hood: Stacks deployed to built infrastructure programmatically 

#### Terraform

IaC tool, start by defining infrastructure in Terraform code and plan then apply the infrastructure on multiple cloud platforms like Azure, AWS, Google Cloud, etc..

**- Strength:** Multi-Cloud support, State tracking with the same configuration

#### How to choose IaC Tools?
**-Criteria:**
  + Plan using one Cloud or many?
  + Role as Developer or Ops?
  + Does the Cloud and Ecosystem support the tool?

### Container Services on AWS

#### Dockerfile

A Dockerfile defines how to build a container image, which describe the environment, dependencies, build steps, and final runtime configuration, ensuring that the application run consistently across any system that support Dockers

**- Images:** A packaged blueprint of an application, build from a Dockerfile using layered file system, used to create containers consistently across environments

**- Workflow:** Dockerfile build a Docker Image which can be used to run Container and push to ECR/Docker Hub

#### Amazon ECR

A fully managed container registry that make it easy to store, manage, and securely share Docker container image.
AWS's own secure and scalable private container registry

**-Features:** 
  + Image Scanning
  + Immutable Tags
  + Lifecycle Policies
  + Encryption & IAM

**- Orchestration:** Orchestrate many containers processes: restart containers, scale up automatically under high load, distribute traffic efficiently, manage where containers are placed and run

#### Kubernetes
Open source, automates deployment, scaling, healing, and load balancing
**- Components:**
  + Master Node: Control Plane, manage worker nodes and pods
  + Worker Node: Run application workloads inside pods
  + Pod: Smallest deployable unit, can contain one or more containers
  + Service

ECS vs EKS

| Feature | Amazon ECS (Elastic Container Service) | Amazon EKS (Elastic Kubernetes Service) |
| :--- | :--- | :--- |
| **Core Technology** | AWS-native container orchestration | Kubernetes-based (open-source standard) |
| **Complexity** | Simpler, easier to operate | Highly flexible but more **complex** |
| **Knowledge Required** | **No Kubernetes knowledge needed** | Requires **Kubernetes knowledge** (pods, deployments, etc.) |
| **AWS Integration** | Deep AWS integration (ALB, IAM, CloudWatch, etc.) | Standard Kubernetes integration |
| **Use Case/Benefits** | Great for **fast deployments** & **lower ops overhead** | **Multi-cluster**, **multi-cloud portability** |
| **Ecosystem/Community** | AWS-native tools and community | **Larger ecosystem** & community tools |
| **Summary** | ECS = easier, faster to run, **lower operational overhead** | EKS = more flexibility, more control, **more complexity** |

#### App Runner

Suitable for quick deployment of web applications and REST APIS, ideal for small to medium production workload

### Monitoring & Observability

#### CloudWatch
- Monitor AWS Resources and Applications running on AWS in real time
- Provide observabilty
- Alarms and automated responses
- Dashboard to help with operational and cost optimization

**- CloudWatch metrics:** Data of the performance of system on AWS or on premise with CloudWatch Agent, integrate well with EventBridge, Auto Scaling and DevOps workflow

#### AWS X-Ray
**- Distributed Tracing:** Tracks requests end-to-end, and draw maps and paths between service visited, add SDK to code to trace IDs

**- Performance Insight:** Root cause analysis for latency and errors, deduce insights from traces and provide Real User Monitoring

### Event Experience

This event was very important for our project as it tackle our plan of adding IaC using CDK, instead of using ClickOps for maintainability and reproducibility. Also some more insights on CloudWatch helped greatly with our data monitoring feature

The speakers answered our team's question:

- Q: Our project up until now have been purely built with ClickOps, and we are planning to use CDK. Are there any tool that could scan and turn our existing infrastructure into CDK or CloudFormation rather than reproducing the infrastructure from scratch with IaC? 
- A: Unfortunately no, there isn't a tool that can assist with that problem yet, your team is going to have to built the infrastructure from scratch again. If there by any chance that you do found a tool that can assist with please share with us too.

- Q: We noticed that AWS X-Ray used with CloudWatch is similar to CloudTrail in its tracing method, can you explain more on what differentiate them?
- A: X-Ray is used for CloudWatch and used to trail the resources and services that the system interacted with, meanwhile CloudTrail is commonly used to trail the AWS user's actions

- Q: Our project is built around Guard Duty Findings, do you have any experiences on how to reliably trigger Findings for a demo scenarios?
- A: In my experience I know that Guard Duty Findings can be triggered by port scanning activities but i'm sure there are other ways too
- A: Guard Duty can be configured to have a threat list containing custom rules to trigger findings upon activities relating the configured malicious domains or IPs  

This event is also the first time some of the speaker's first time presenting a topic:
- The DevOps and IaC sections was well presented 
- Monitoring & Observability wasn't as great and we can notice the speaker's nervousness but still delivered great values regardless

#### Some event photos
![Group picture during the event taken by speaker Tran Dai Vi](/images/4-Event/CM2GroupPic.jpg)
_Group picture by speaker Tran Dai Vi_