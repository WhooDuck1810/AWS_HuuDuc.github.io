---
title: "Configure Route 53 DNS & ACM SSL Certificate"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1 Configure Route 53 DNS & ACM SSL Certificate

In this step, map your custom domain (purchased externally) to AWS Route 53 and register an SSL/TLS Certificate in AWS Certificate Manager (ACM).

### 1. External Domain Registration & Route 53 Hosted Zone
- The custom domain `trackermaint.dpdns.org` was acquired from an external domain registrar.
- A Public Hosted Zone for `trackermaint.dpdns.org` was created in **AWS Route 53**, and custom NS (Name Server) records were configured at the domain registrar.
- Created an **A Record** (Alias) pointing traffic directly to the Application Load Balancer / Elastic IP.

<div style="text-align: center; margin: 20px 0;">

  ![Route 53 Records](/images/5-Workshop/5.6-Cleanup/5.6.1-route53-acm/route53-records.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.6.1. Route 53 Hosted Zone records for trackermaint.dpdns.org showing A Record, NS, SOA, and CNAME validation.</div>
</div>

### 2. AWS Certificate Manager (ACM) SSL Certificate
- Requested/Registered a public SSL/TLS Certificate for `trackermaint.dpdns.org` in **AWS Certificate Manager (ACM)**.
- Performed CNAME DNS validation via Route 53 to verify domain ownership.
- **Certificate Status:** **Issued** (In use by HTTPS endpoints) ✅

<div style="text-align: center; margin: 20px 0;">

  ![ACM Certificate Status](/images/5-Workshop/5.6-Cleanup/5.6.1-route53-acm/acm-certificate.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.6.2. AWS Certificate Manager (ACM) dashboard displaying Issued status for trackermaint.dpdns.org.</div>
</div>
