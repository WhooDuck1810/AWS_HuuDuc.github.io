---
title: "Provision Amazon RDS PostgreSQL Database"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

# 5.3.4 Provision Amazon RDS PostgreSQL Database

In this step, deploy a managed PostgreSQL database inside the Private Subnet.

1. Open the **Amazon RDS Console** => Click **Create database**.
2. Choose **Standard create** => Engine: **PostgreSQL** (Version 15.x).
3. Template: **Free Tier** => Instance class: `db.t4g.micro`.
4. Settings: DB instance identifier `tracker-maintenance-db`, Master username `postgres`.
5. Connectivity: Network `Tracker-VPC`, Subnet group in Private Subnet, Public access: **No**.
6. Security group: Choose `tracker-rds-sg`.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS RDS Console screenshot showing the `tracker-maintenance-db` details and Endpoint connection string here.
> 
> ![RDS Database Setup](/images/5-Workshop/5.3-S3-vpc/rds-db-setup.png?classes=shadow)
