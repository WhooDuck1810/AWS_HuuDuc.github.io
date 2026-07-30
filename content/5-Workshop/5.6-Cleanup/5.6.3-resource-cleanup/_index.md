---
title: "Resource Clean Up Guide"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

# 5.6.3 Resource Clean Up Guide (Stop vs Terminate)

Follow these steps to temporarily pause or permanently delete AWS resources to prevent unexpected billing.

- **Stop EC2:** Preserves configuration, incurs $0 compute cost (EBS storage only).
- **Stop RDS:** Pauses PostgreSQL DB for up to 7 days.
- **Delete S3 & ECR Objects:** Empties uploaded photos and old Docker image tags.
- **Permanent Termination:** Delete EC2, RDS, S3, ECR, Route 53, ACM, and IAM Roles.
