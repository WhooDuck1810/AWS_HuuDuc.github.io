---
title: "Configure Security Groups for EC2 & RDS"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3 Configure Security Groups for EC2 & RDS

In this step, configure two virtual firewall security groups to enforce isolation between the public web instance and the private database.

### 1. Inbound Rules Configuration
- **SSH (22):** `0.0.0.0/0` (Remote access)
- **HTTP (80):** `0.0.0.0/0` (Public web traffic)
- **HTTPS (443):** `0.0.0.0/0` (Secure web traffic)
- **Custom TCP (3000):** Frontend React App container
- **Custom TCP (8081):** Backend Spring Boot API container

### 2. Outbound Rules Configuration
- **All Traffic:** `0.0.0.0/0`
- **PostgreSQL (5432):** Routed to RDS Security Group (`ec2-rds-1` / `sg-0c0bf5f62fdf25148`)

<div style="text-align: center; margin: 20px 0;">

  ![Security Groups Rules](/images/5-Workshop/5.3-S3-vpc/5.3.3-security-groups/security-groups-rules.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.3.4. Inbound and Outbound Security Group rules for EC2 server and RDS database connection.</div>
</div>
