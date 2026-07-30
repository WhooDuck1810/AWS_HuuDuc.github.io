---
title: "Configure Route 53 DNS & ACM SSL Certificate"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1 Configure Route 53 DNS & ACM SSL Certificate

In this step, map the custom domain to the EC2 Elastic IP and request an ACM SSL certificate.

1. Open **Route 53 Console** => Create Hosted Zone for `trackermaint.dpdns.org`.
2. Create an **A Record** pointing `trackermaint.dpdns.org` to the EC2 Elastic IP (`3.106.194.112`).
3. Request an **ACM Certificate** in `us-east-1` (N. Virginia) for `trackermaint.dpdns.org` to prepare for CloudFront integration.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS Route 53 Console screenshot showing the A Record details here.
> 
> ![Route 53 Setup](/images/5-Workshop/5.3-S3-vpc/route53-acm-setup.png?classes=shadow)
