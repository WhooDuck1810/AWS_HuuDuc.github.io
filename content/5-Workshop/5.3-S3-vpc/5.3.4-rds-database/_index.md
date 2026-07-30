---
title: "Provision Amazon RDS PostgreSQL Database"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

# 5.3.4 Provision Amazon RDS PostgreSQL Database

In this step, deploy a managed PostgreSQL relational database instance inside the Private Subnet of `Tracker-VPC`.

### Database Specifications
- **DB Instance Identifier:** `tracker-maintenance-db`
- **Database Engine:** PostgreSQL 15.x
- **Instance Class:** `db.t3.micro`
- **Status:** **Available** ✅
- **Region & AZ:** `ap-southeast-2b` (Sydney)
- **Public Access:** Disabled (Private Subnet only)

<div style="text-align: center; margin: 20px 0;">

  ![RDS Database Summary](/images/5-Workshop/5.3-S3-vpc/5.3.4-rds-database/rds-summary.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.3.5. Amazon RDS PostgreSQL instance summary showing Available status and private connectivity settings.</div>
</div>
