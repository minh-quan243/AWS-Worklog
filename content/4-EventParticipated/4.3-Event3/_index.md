---
title: "Event 3"
date: "2025-11-15"
weight: 03
chapter: false
pre: " <b> 4.3. </b> "
---

# Summary Report: “AWS Cloud Mastery Series #1 - AI/ML/GenAI on AWS”

### Event Objectives

- Introduce AI/ML/GenAI on AWS

### Speakers

- **Lam Tuan Kiet** – Sr DevOps Engineer, FPT Software
- **Danh Hoang Hieu Nghi** - AI Engineer, Renova Cloud
- **Dinh Le Hoang Anh** - Cloud Engineer Trainee, First Cloud AI Journey
- **Van Hoang Kha** - Cloud Security Engineer, AWS Community Builder
### Key Highlights

# Explored Generative AI with Amazon Bedrock:

**- Foundation Models:** Different from Traditional Model in a sense that it can be adapted for many tasks, provided many fully managed model from leading AI Companies such as: OpenAI, Claude, Anthropic, etc.  
**- Prompt Engineering:** Crafting and Refining Instructions
   + Zero-Shot Prompting: A prompt with no prior context or example
   + Few-shot Prompting: A prompt with a few prior context and example
   + Chain of Thought: A prompt with thought processes and steps for the actual answer 
**- Retrieval Augmented Generation(RAG):** Retrieving relevant information from a data source
   + R: Retrieval - Retrieves relevant information from a knowledge base or data sources 
   + A: Augmented - Adding the information retrieved as additional context in the user's prompt before inputting it into the model
   + G: Generation - Responses from the model for the augmented prompt
   + Use cases: Improved content quality, contextual chatbots and question answering, personalized search and real time data summarization

**- Amazon Titan Embedding:** Lightweight model that excels in translating text into numerical representations(embeddings) for high accuracy retrieval tasks, with support for 100+ languages

**- Pretrained AI Services:** 
  + Amazon Rekognition: Image and Video analysis
  + Amazon Translate: Detect and translate text
  + Amazon Textract: Extract Texts and Layouts from documents
  + Amazon Transcribe: Speech-to-text
  + Amazon Polly: Text-to-speech
  + Amazon Comprehend: Extract Insights and Relationships from text
  + Amazon Kendra: Intelligent Search Service
  + Amazon Lookout: Detect Anomalies in business metrics, equipment and images
  + Amazon Personalize: Tailor recommendations to user 

**- Demo:** AMZPhoto: Face recognition from images using AI

**- Amazon Bedrock AgentCore:** A comprehensive agentic platform designed to address challenges in bringing agents into production:
  + Securely execute and scale agent code.
  + Incorporate memory (remembering past interactions and learning).
  + Implement identity and access controls for agents and tools.
  + Provide agentic tool use for complex workflows.
  + Discover and connect with custom tools and resources.
  + Understand and audit every interaction (observability).
  **+ Foundational Services:** These services are categorized for running agents securely at scale.

  **+ Enhance with tools & memory:** Includes Memory, Gateway, Browser tool, and Code Interpreter.

  **+ Deploy securely at scale:** Includes Runtime and Identity.

  **+ Gain operational insights:** Includes Observability.

  **+ Enabling Agents at Scale (Architecture):** Connects to the AgentCore Gateway (via MCP), Memory, Identity, Observability, Browser, and Code Interpreter.

  **+ Frameworks for Building Agents:** CrewAI, Google ADK, LangGraph/LangChain, LlamaIndex, OpenAI Agents SDK, and Strands Agents SDK.

### Key Takeaways
- Bedrock is the GenAI Hub: Amazon Bedrock provides fully managed Foundation Models from top companies for many different tasks.

- Customization via Prompts and Data: Various ways to prompts (Zero-Shot, Few-shot, CoT) and utilize RAG to add info for better model responses.

- Embeddings Power Search: Amazon Titan Embedding is a key lightweight model for translating text to numbers, which helps achieve high accuracy in retrieval tasks (like RAG).

- Pretrained Models: AWS offers many ready-to-use AI services for common needs, like Rekognition for images and Textract for documents.

- AgentCore Solves Production Issues: Amazon Bedrock AgentCore is the new comprehensive platform that handles the difficult parts of running AI Agents at scale (like Memory, Identity, and Observability).

### Applying to Work

- Very useful in our team's later projects which could include more usage of AI Foundation Models in our architecture.

### Event Experience
- The speakers are very well spoken and informative
- Q&A: Team member asked an out of topic question but crucial to our project 
  + Q: The SNS in our architecture which is used to process Guard Duty Findings have encountered a situation where over 1000+ alerts appear at once, how would we resolve this?
  + A: Add SQS to queue the events and ensure no alert is to be missed
- Placed top 10 in the end of event Kahoot Quiz and got a picture with the speakers 
- Created an unofficial group: "Mèo Cam Đeo Khăn", a joint collaboration between my group "The Ballers" and "Vinhomies" 

#### Some event photos
![Top 10 Kahoot](/images/4-Event/Kahoottop10.jpg)
_Top 10 Kahoot_
![A Picture with the speakers and top 10 players](/images/4-Event/WithSpeaker.jpg)
_Picture with top 10 players_
![Mèo Cam Đeo Khăn group](/images/4-Event/Meocamdeokhan.jpg)
_Meo Cam Dao Khan group picture_