---
title: "Route 53 DNS"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.3.2. </b> "
---

Amazon Route 53 provides DNS for the Voice Summarizer platform's custom domain. In this section you will create a **Public Hosted Zone** for your domain, delegate it to Route 53's nameservers via your domain registrar, then create an **alias A record** that routes your domain to the Application Load Balancer.

Once complete, users will reach the platform via your custom domain (e.g. `voice.yourdomain.com`) rather than the raw ALB DNS name.

{{% notice info %}}
**Prerequisites for this section:**
- You must own a domain registered with any domain registrar (e.g. Namecheap, GoDaddy, Google Domains, or AWS Route 53 Registrar).
- The **Application Load Balancer** must already be created (Section 7), as you will need its DNS name for the alias record.
{{% /notice %}}

---

#### Step 1 — Go to Hosted Zones and Create

Navigate to **Route 53** in the AWS Console. In the left sidebar click **Hosted zones**, then click **Create hosted zone**.

![Go to Hosted Zones and Create](/images/4-Workshop/Route53/B1_Goto_Route53.png)

---

#### Step 2 — Enter Your Domain Name and Choose Public Hosted Zone

Fill in the Hosted Zone settings:

- **Domain name**: Enter your custom domain, e.g. `yourdomain.com` or `voice.yourdomain.com`
- **Type**: **Public hosted zone** *(resolves DNS queries from the public internet)*
- Leave all other settings as default

Click **Create hosted zone**.

![Enter Domain Name — Public Hosted Zone](/images/4-Workshop/Route53/B2_Enter_Domain.png)

After creation, Route 53 automatically generates two records inside the Hosted Zone:
- An **NS** record containing four Route 53 nameservers
- An **SOA** record

---

#### Step 3 — Copy the NS (Nameserver) Values

Click on the newly created Hosted Zone to open it. Locate the **NS** record and copy all four nameserver values. They will look similar to:

```
ns-123.awsdns-45.com.
ns-678.awsdns-90.net.
ns-111.awsdns-22.org.
ns-444.awsdns-88.co.uk.
```

You will paste these into your domain registrar's nameserver settings in the next step.

![Copy NS Values](/images/4-Workshop/Route53/B3_Copy_Value_Route.png)

---

#### Step 4 — Add the NS Records to Your Domain Registrar

Log in to your domain registrar's control panel (e.g. Namecheap, GoDaddy, or wherever the domain is registered). Navigate to the DNS or Nameserver settings for your domain.

Replace the existing nameservers with the four Route 53 NS values you copied in Step 3. Save the changes.

{{% notice warning %}}
⚠️ DNS propagation after changing nameservers can take anywhere from a few minutes to 48 hours, depending on your registrar's TTL settings. You can monitor propagation using tools like [dnschecker.org](https://dnschecker.org).
{{% /notice %}}

![Add NS Records at Registrar](/images/4-Workshop/Route53/B4_Add_NS_Record_To_Ur_Domain.png)

---

#### Step 5 — Create an Alias A Record Pointing to the ALB

Back in the Route 53 Hosted Zone, click **Create record** to add an alias record that maps your domain to the Application Load Balancer.

Configure the record as follows:

- **Record name**: Leave blank for the apex domain (`yourdomain.com`), or enter a subdomain such as `api` for `api.yourdomain.com`
- **Record type**: **A — Routes traffic to an IPv4 address and some AWS resources**
- **Alias**: ✅ **Toggle on**
- **Route traffic to**: **Alias to Application and Classic Load Balancer**
- **Region**: `ap-southeast-1`
- **Load balancer**: Select `voice-summarizer-alb` from the dropdown

Click **Create records**.

![Create Alias A Record to ALB](/images/4-Workshop/Route53/B5_Create_Alias_Pointing_To_ALB.png)

{{% notice info %}}
**Two records for two endpoints:** If you want to serve both the frontend and the API from the same domain using subdomains, create two alias records:<br>• Apex or `www` subdomain → Amplify App domain (find this in Amplify Console → App settings → Domain management)<br>• `api` subdomain → ALB DNS name<br>For this workshop a single alias record pointing to the ALB is sufficient if the frontend is accessed directly via the Amplify default URL.
{{% /notice %}}

---

{{% notice tip %}}
✅ Route 53 is configured. Your domain now resolves through Route 53, and traffic to your custom domain is routed to the Application Load Balancer.<br>
**To verify:** Run `nslookup yourdomain.com` or `dig yourdomain.com` from your terminal. The answer should return the ALB's IP addresses once propagation is complete.
{{% /notice %}}
