---
title: "Difficulties Encountered & Future Roadmap"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.6.4. </b> "
---

# 5.6.4 Difficulties Encountered & Future Roadmap

### 1. Difficulties Encountered
- **Windows vs Linux Paths:** Resolved by moving React Router builds to GitHub Actions Linux runners.
- **EC2 RAM Limits:** Resolved by adding `-Xms256m -Xmx512m` JVM memory caps in Docker Compose.
- **CloudFront Verification:** Certificate issued, pending AWS account verification.

### 2. Future Development Roadmap
- **Infrastructure as Code (AWS CDK v2):** Automate 100% infrastructure setup via TypeScript code.
- **Multi-Region Active-Active:** Expand EC2 instances to Singapore & Ohio leveraging ECR replication.
- **Aurora Global Database:** Upgrade from RDS to Aurora PostgreSQL for global read replicas.
- **Auto Scaling & ALB:** Deploy Auto Scaling Group behind Application Load Balancer.
