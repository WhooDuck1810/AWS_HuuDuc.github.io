---
title: "Configure Security Groups for EC2 & RDS"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3 Configure Security Groups for EC2 & RDS

In this step, create two virtual firewalls in `Tracker-VPC` to enforce strict isolation.

1. **EC2 Security Group (`tracker-ec2-sg`):**
   - **Inbound Rules:**
     - SSH (22) => Your IP
     - HTTP (80) => `0.0.0.0/0`
     - HTTPS (443) => `0.0.0.0/0`
     - Custom TCP (3000) => `0.0.0.0/0` (Frontend React)
     - Custom TCP (8081) => `0.0.0.0/0` (Backend Spring Boot API)
2. **RDS Security Group (`tracker-rds-sg`):**
   - **Inbound Rules:**
     - PostgreSQL (5432) => Source: `tracker-ec2-sg` (Security Group ID).

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS Console screenshot showing the Inbound Rules for `tracker-ec2-sg` and `tracker-rds-sg` here.
> 
> ![Security Groups Setup](/images/5-Workshop/5.3-S3-vpc/security-groups-setup.png?classes=shadow)
