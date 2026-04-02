---
title: "Event 5"
date: "2025-11-19"
weight: 05
chapter: false
pre: " <b> 4.5. </b> "
---

# Summary Report: “Secure Your Applications: AWS Perimeter Protection Workshop”

### Event Objectives

- Introduce CloudFront as foundation for perimeter protection
- Introduce WAF and Application Protection
- Hands On Workshop: Optimize web application with CloudFront
- Hands On Workshop: Secure Internet Web Application

### Speakers

- **Nguyen Gia Hung** - Head of Solution Architect
- **Julian Ju** - Senior Edge Services Specialist Solution Architect
- **Kevin Lim** - Senior Edge Services Specialist GTM

### Key Highlights

### CloudFront

#### What Form Of Security Do Customers Need And The Solution

Customer Personas:
- Small website owner: Basic security protection
- Business users: Comprehensive security against DDoS and bots
- Scaling businesses: Advanced configurability like origin failover and origin load reduction

Solution: CloudFront
- Security with predictable monthly cost: Pay flat prices for CDN, WAF, DDoS Protection, DNS and Storage
- Single plan selection

#### New CloudFront Pricing: CloudFront Flat-Rate

- Flat pricing for CDN, WAF, DDoS Protection, DNS and Storage, pay the same price for unlimited usage

- AWS provides 4 flat pricing plans for CloudFront:
  + Free: $0/Month
  + Pro: $15/Month
  + Business: $200/Month
  + Premium: $1000/Month

- Each plan have its own usage limit per month:
  + Free: 1 million requests + 100 GB Data
  + Pro: 10 millions requests + 50 TB Data
  + Business: 125 millions requests + 50 TB Data
  + Premium: 500 millions requests + 50 TB Data

- Exceeding the usage limit => No extra cost but CloudFront will reduce performance and send alerts  

#### How CloudFront helps in perimeter protection
- Fight distributed attacks with distributed defense: Rather than defending attacks from all over the world at once, CloudFront helps by defending at edge locations nearest to the source of attacks
- Shield Advanced: Gains visibility into infrastructure-layer attacks, access to Shield Response Team 24/7
-  Volumetric attack: CloudFront offer inline mitigation, STN Proxy protect from SYN flood attack and automatic network routing

#### Cost optimization with Amazon CloudFront

- AWS Data transfer out to CloudFront: Free
  
- HTTP compression: ~80% compression of object file
  
- HTTPS Encryption in transit:
    + AWS proprietary TLS
    + Automatic TLS certificate issuance and validation for no additional cost
    + Automatic redirection from HTTP to HTTPS
    + TLS version and cipher suite managed by security policy
    + TLS 1.3, ECDSA cert, post quantum
  
- Upcoming mutual TLS support
  
- Origin cloaking with Origin Access Control: signs request with short term credential, applies to S3 Bucket, Lambda Function URL, Elemental Media Package

- Origin cloaking for custom origins: Only allow CloudFront IP with managed prefixes list and add custom header with a predefined secret

- Content protection with Signed URL: Prevent URL Copy Paste theft

- Caching benefits for improved availability: Cache content based on TTL, respond without request to origin and respond stale content in case of origin timeout

- Built-in failover and origin failover: Automatically route traffic to optimal and healthy edge location, same goes with origin when the primary fails=> failover to secondary origin

- Graceful failure: Support for custom error pages, stale content and cached error result

#### Enhanced performance with Amazon CloudFront

- Multi-layer caching architecture: Aggregates traffic from CloudFront PoPs in Regional Edge Caches and Origin Shield, enable request collapsing and improve cache hit ratio

- Multiplexing: Download in parallel

- Persistent connections to origins: Avoid extra TCP handshake and maintain full TCP capacity

- AWS Global backbone: Lower Latency and no public internet event impact

- Run logic at the edge: 
    + Redirect at edge (CloudFront Function, Lambda at Edge)
    + URL Rewrite / AB Testing / Geographic Redirect Device-based Redirect
    + Device based content delivery
    + Respond faster without origin: Rate limiting / API Mocking / Health check / Error Handling

#### CloudFront use cases

- Static web resources: High cache hit ratio => Performance gains + Minimal origin load

- Whole website delivery: Global performance + Security + High availability + Cost 
optimization

- API acceleration: Reuse connection + AWS backbone => Reduce latency + Selective caching

- Media Streaming: Unlimited scale + Low latency + Cost-effective + Millions of concurrent user

- Large file download: Range requests + Edge caching + 80-90% cost savings

#### CloudFront best practices

- End to end visibility: Real user/Internet/Infrastructure monitoring

- Maximize caching: Normalize cache key, cache dynamic/private content and error caching

- Block unwanted requests: Malicious pattern, rate limiting, DDoS mitigation and filter API requests

- Offload business logic: CORS response, redirection at edge and set cookies

- Automatic failover: Route 53 failover routing + CloudFront Origin Group for request level failover

### AWS WAF & Application Protection

#### Threats and impact to business

- Denial of Service

- App Vulnerabilities

- Bot traffic

**=>** Stolen data, compromised credentials, spam, downtime, manual intervention, raise infrastructure cost, losing credibility

#### Surge in bot activities

Diverse AI bots from a wide range of source significantly increased in customer environment: 155% increase YoY

#### Route 53 in perimeter protection

Globally dispersed DNS servers => Automatic scaling and DDoS mitigation + Availability SLA 100%

#### Infrastructure Protection with AWS Shield

- At edge:
    + SYN Proxy
    + Inspection
    + Packet validation
    + Distributed scrubbing capacity
    + Automatic Routing policy

- At border:
    + Traffic filtering
    + Resource-level detection and mitigation
    + Health-based detection

#### Inspecting HTTP with AWS WAF

- WAF works together with CloudFront

- Detect HTTP flood, malicious patterns, bad IP

- Bot control

- Fraud control

#### Shield Advanced Incident Response

- Shield/Shield Advanced has metric to trigger alarm to start incident response

- Shield Advanced provides Shield Response Team 24/7

- Find attack vector and contributor

#### WAF Configuration

- Add rules and rules group

- Use managed rules and custom rules

- Start with COUNT mode: Monitor to avoid false positives

- Use labels to customize logic and responses

- Scope-down to optimize cost

#### Web ACL/Protection Pack

- Set of rules, rule groups and default action

- Associated to resources like CloudFront Distribution

- Logging and sampling configurations

#### WAF Rules

- Inspection criteria (IP Address/Header value/Request Body) => Action (Allow/Block/Count/Custom)

- Rate-based rule: Based on number of HTTP request from one IP

#### WAF Anti-DDoS Application Layer Protection

- Automatic application layer DDoS mitigation

- Protection with pre-configured rules

- Configurable DDoS Protection based on application needs

#### WAF Labels

Added by managed rules (Always) and custom rules (Optional): Indicate matched rule, session status, Geographic and IP-based data and Bot/Fraud activities

#### Common bots

What: Self-identifying or Search Engines, Social media, HTTP library
Common bot control: Looks for request, IP, TLS fingerprint and verifies the bot with labels

#### Evasive bots

What: Scrapers, credential stuffers, vulnerability scanners, use existing browser headers and values, not well known and mimic real common bots
Solution: Client interrogation, identify unique client session and monitor its behavior

### CloudFront hands-on: Perimeter Protection Workshop

#### Comparison between normal static S3 hosting Origin and S3 hosting with CloudFront

- CloudFront delivers an object faster when its served from cache

- CloudFront delivers an object faster by using the AWS global network instead of the public internet

- In CloudFront, compression is applied, the object size is significantly reduced

#### AWS WAF hands-on: Strengthen Your Web Application Defenses with AWS WAF 

- Created WAF rules to defend against:
  + Cross site Scripting (XSS)
  + SQL Injection
  + Path traversal attack
  + Server-side asset
  + Common/Evasive Bot
  + API misuse
  + Mystery Test: Configured rule to block an access with encrypted header value

### Event Experience

The event was very informative and right on time as our team was setting up CloudFront the night before. The new pricing system is so much better for us, with additional WAF for security

The workshop was very interactive and fun, with Mr.Julian being as supportive as possible too
We got our questions answered by Mr.Julian after the event too:

Q: WAF can be used control and block bot activities, but lately but the improvement of AI: Especially Gemini's new Agent, which can directly interact on our browser, not headless, can WAF help protect against those?

A: Well with Gemini, the interractions are still called by an API so WAF can detect that, but there is another that we cant detect, Claude Agent, but its only an accessibilty tool and doesnt do many harmful things normal bots do so in the near future we wont need to concern it yet 

#### Some event photos
![Attendee](/images/4-Event/Event5AttendeePic.jpg)
_Event Attendee Group Picture_

![Top24Kahoot](/images/4-Event/Event5KahootTop24.jpg)
_Placed Top 24 on end of event Kahoot Quiz_

![Full Threat Blocked](/images/4-Event/FullThreatBlockedWAFWorkshop.jpg)
_Blocked all workshop's threats_