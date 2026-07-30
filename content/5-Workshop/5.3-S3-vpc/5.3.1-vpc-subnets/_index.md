---
title: "Create Custom VPC & Subnets"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1 Create Custom VPC & Subnets

In this step, you will initialize an isolated virtual private network for the maintenance application.

1. Open the **Amazon VPC Console** => Click **Create VPC**.
2. Select **VPC only** => Set Name tag to `Tracker-VPC` => Set IPv4 CIDR block to `10.1.0.0/16`.
3. In the navigation pane, choose **Subnets** => Click **Create subnet**:
   - **Public Subnet:** Name `tracker-public-subnet-1`, CIDR `10.1.1.0/24`, Availability Zone `ap-southeast-2a`.
   - **Private Subnet:** Name `tracker-private-subnet-1`, CIDR `10.1.2.0/24`, Availability Zone `ap-southeast-2b`.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS VPC Console screenshot showing the `Tracker-VPC` details and Subnet allocation here.
> 
> ![VPC and Subnet Setup](/images/5-Workshop/5.3-S3-vpc/vpc-subnet-setup.png?classes=shadow)
