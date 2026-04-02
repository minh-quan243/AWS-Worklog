---
title: "Event 7"
date: "2025-11-29"
weight: 07
chapter: false
pre: " <b> 4.7. </b> "
---

# Summary Report: “BUILDING AGENTIC AI - Context Optimization with Amazon Bedrock”

### Event Objectives

- Technical deep-dive into AWS Bedrock Agent Core
- Building Agentic Workflow on AWS
- Introduction to Diaflow
- Introduction to CloudThinker
- CloudThinker Agentic Orchestration, Context Optimization on Amazon Bedrock
- CloudThinker Hack: Hands-on Workshop

### Speakers

- **Nguyen Gia Hung** - Head of Solutions Architect AWS

- **Kien Nguyen** - AWS Startup Solutions Architect

- **Viet Pham** - CEO & Founder of Diaflow

- **Thang Ton** - Co-Founder & COO CloudThinker

- **Henry Bui** - Head of Engineering CloudThinker
 
- **Van Hoang Kha** - AWS Comumnity Leaders

- **Nhat Tran** - CTO CloudThinker

### Key Highlights

### Amazon Bedrock AgentCore
- The evolution to Agentic AI: The number of enterprise ultilizing Agentic AI systems is on the rise 
=> AWS provide the best place to build and scale AI Agents

What does AWS provides?
- Frameworks for building agents: Strands, Langgraph, OpenAI, etc..
- Applications: Kiro, AWS QuickSuite
- Amazon Sagemaker
- Amazon Bedrock Models and AgentCore

What are Amazon Bedrock AgentCores?
- Runtime: Secure, serverless agent deployment => Scale workloads and accelerate time to market
- Identity: Enterprise agent identity and access management => Accelerate development and secure access for AI Agents and build streamlined AI Agent experiences
- Gateway: Unified, secure tool access => Simplified development and secure access of tools, with intelligent tool discovery
- Memory: Intelligent context-aware memory retention => Simplify memory management of enterprise-grade data with deep customization
- Browser: Scalable cloud browser runtime: Provides serverless browser infastructure with enterprise-grade security and observabililty
- Code Interpreter: Secure sandboxed code execution => Execute code securely, provide large-scale data processing and ease of use
- Observability: Complete agent performance visibility => Maintain quality of trust and accelerate time to market, allow intergration with other observability tools

### Diaflow: Amplify team's potential with AI Automation
Diaflow is an AI Automation Platform for Businesses whose mission is to unlock the full potential of teams by providing powerful, flexible tools to automate what matters most—fast and without code

- Transforms manual processes into intelligent workflows in minutes, not months => solving 20+ hours/week/person wasted on repetitive tasks in 90% of businesses, and concerns over data privacy and security when deploying AI internally

- Customers: 6000+ users from global customers across various industries, including Retail, IT Services, Finance, Marketing, and Healthcare.

- Backed By: Developed by experts from major tech and academic institutions and sponsored/backed by key partners like NVIDIA, Microsoft, AWS, and Google.

- Locations: The USA, Switzerland, France, South Korea, Singapore, and Vietnam.

#### Diaflow Capabilities

- AI-Powered Automation: Reduces manual tasks by 80% by connecting Databases, Apps, Knowledge bases, APIs, and Legacy systems

- AI Agents: Deploys 24/7 intelligent assistance 

- Internal AI Tools & Apps: Deploy AI-powered apps tailored for specific business needs.

#### Diaflow Features

- Autonomous Task Execution: Describe your goal, Diaflow will plan and execute it

- Multi-Model Processing: Choose from Claude, Gemini, Deepseek, etc..

- Intergration with usable tools: Built-in Drive, Pages, Table and Vector

- Enterprise-Grade Security:  HIPAA, SOC2 and GDPR

### CloudThinker

Some problems for customers using cloud platforms: Exploding Cloud Cost, Cloud Complexity > Cloud Capability and Reactive Incident Response

#### CloudThinker Capabilities

Agentic AI Cloud Operations: Insights -> Reasoning -> Execution
- Multi-agent Collaboration
- Autonomous Operations
- Continuous Optimization
- Continuous Learning
- Multi-cloud Capability

#### CloudThinker Product Feature

- Code review
- Incident Response
- Operation Automation
- Infastructure Observability
- Kubernetes Management (Coming soon)
- Cloud Keeper (Coming soon)

#### CloudThinker Solutions

- Cloud Migration & Modernization
- Cloud Operation & Optimization
- Compliance Readiness
- Security Assessment

#### CloudThinker AI Agent Team

- Alex: Cloud Engineer
- Kai: Kubenetes Engineer
- Anna: General Manager
- Oliver: Security Engineer
- Tony: Database Engineer

### CloudThinker Agentic Orchestration and Context Optimization on Amazon Bedrock

#### AI Agent Evolution and Architecture

- Chatbots: Reactive, use rule-based decisions for simple tasks, and have limited adaptability.

- Agents: Proactive, capable of initiating actions, making dynamic and complex decisions, and adapting by learning from experience to handle complex, multi-step tasks.

- Core Agent Core Architecture Components:
    + Planning: Breaks down tasks and creates steps.

    + Memory: Recalls context and interactions.

    + Tools: Uses external APIs, search, and code.

    + Action: Executes the plan and provides the final response.

#### Getting started with Agents

- ReAct Agent Architecture: Interleaving thought and action for complex tasks => User Input → Reasoning (Thought) → Action (Tool Use) → Observation → Final Answer.

- Evals & Observability from Day 1

- Utilize tools: Effective tools have Agent Context, diverse workflows, intuitive problem solving and have real-world tests

- Quick win techniques: Prompt Caching -> Context Compaction -> Tool Consolidation -> Parallel tool calling

#### Multi-Agent vs Multi Session: Solving Single-Agent Bottlenecks

Single-Agent systems: which handle all tasks monolithically, face issues like context isolation (100k+ tokens), sequential processing, and multi-batch problems. This leads to two patterns for distributed orchestration: Multi-Agent and Multi-Session.

#### Agent Coordination Models

 | Feature | Network (Peer-to-Peer) | Supervisor |
| :--- | :--- | :--- |
| **Structure** | Agents communicate directly with each other. | A central Supervisor delegates tasks to specialized Worker agents. |
| **Pros** | * Flexible communication. * Good fault tolerance. * Easier to switch from single-agent to multi-agent modes. | * Clear task delegation. * Central coordination. * Modular and scalable. |
| **Cons** | * Complex debugging. * Ownership ambiguity. | * Bottleneck risk. * Cannot switch to single-agent mode. |

#### Supervisor Variants: Three Approaches
| Approach | Group Chat-based Supervisor | Supervisor as Tool (Subagents) | Hierarchical |
| :--- | :--- | :--- | :--- |
| **Notes** | Collaborative context, simple coordination, **Best for small teams**. **CloudThinker uses this.** | Fine-grained control, less worker autonomy, no direct user access. | Scales to large organizations, multiple supervision layers, more complex setup. |

#### Single-Agent and Multi-Agent in a Unified Architecture:

- Unified architecture that dynamically routes tasks to the appropriate agents: Uses a Supervisor for multi-agent coordination and retry.

- Task routing is determined by complexity and agent capability:
    + Simple requests are sent directly to specialists
    + Complex tasks requiring coordination are transferred to the Supervisor.
    + Simple tasks that an agent can handle alone are managed by that agent.
    + Simple tasks that require collaboration are handled via a Group Chat.
    + A default specialist agent handles requests when no specific agent is mentioned 

#### Solving the 100:1 Agentic Cost Problem

Agentic Architectures face a "Context Explosion" where the Input/Output token ratio can be 100:1, compared to roughly 3:1 for chatbots. Input costs dominate due to 50–100 tool calls per task, compounding context degradation and latency.

=> Four Quick-win Techniques: combined savings of 80–95% and a 3–5x latency reduction:

- Prompt Caching:

    + Goal: Achieves 70–90% cost reduction and 95%+ cache hit rates.

    + Method: Uses a Three-Tier Architecture where the System Prompt (Tier 1) and Conversation History (Tier 2) are cached, while the current turn (Tier 3) is not, preserving turn-by-turn adaptability.

- Context Compaction:

    + Goal: Delivers 80% summarization cost reduction and maintains longer sessions.

    + Method: Cache-Preserving Summarization appends summarization instructions at the payload level to preserve cache keys, summarizing old messages to preserve context without hitting limits.

- Tool Consolidation:

    + Goal: 20% reduction in token count and hallucination, leading to a faster decision time.

    + Tool Pollution Problem: Too many tool definitions (e.g., 50) flood the context window.

    + Method: Consolidates CRUD operations into single interfaces with command-based parameters, using Just-In-Time Schemas to fetch documentation only when needed.

- Parallel Tool Calling:

    + Goal: 30–40% reduction in power round trips and a 3–5x latency reduction.

    + Method: Uses explicit instruction to parallelize tool calls and reasoning steps, which is necessary because modern LLMs can parallelize but won't without explicit guidance.

#### Cross-Region Inference

- Problem: Single-region deployments can hit rate limits due to 50–100 tool calls per task, creating a single point of failure.

=> Solution: Automatically routes workloads across regions (US, EU, APAC) to eliminate rate limit bottlenecks and provide a global profile for near-zero latency and improved throughput.

#### The best multi-agent system self-realizes when NOT to use multiple agents.

### CloudThinker Hack: Hands-On Workshop

- Organizers provide attendees with CloudThinker instructions and Free Standard Plan Code to use on analyzing AWS Accounts

### Event Experience

- The event was very informative and strengthened our understading of Amazon Bedrocks and introduced us to Diaflow and CloudThinker, two incredibly useful AI Agents Tools

- Participation in CloudThinker Hack: Hands-On Workshop: Used CloudThinker to analyze our AWS Account's infastructure => Analyzed our abnormally high amount of S3 Bucket GET requests (3 millions in 2 weeks) and resolve it by recommending us to use Data Firehose to consolidate logs before saving

=> Our team got a prize for using CloudThinker to analyze our problem: CloudThinker T-Shirt and CloudThinker Keychain

#### Some event photos
![Picture of all Attendees](/images/4-Event/Event7AllAttendee.jpg)
_Picture of all attendees_

![Recieving Prize From CloudThinker1](/images/4-Event/CloudThinkerPrize1.jpg)
_Recieving Prize From CloudThinker_

![Recieving Prize From CloudThinker2](/images/4-Event/CloudThinkerPrize2.jpg)
_Recieving Prize From CloudThinker_
