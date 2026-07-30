---
title: "Configure Internet Gateway & Route Tables"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2 Configure Internet Gateway & Route Tables

In this step, connect the Public Subnet to the Internet Gateway.

1. In the VPC Console, choose **Internet Gateways** => Click **Create internet gateway** => Name: `tracker-igw`.
2. Select `tracker-igw` => Click **Actions** => Choose **Attach to VPC** => Select `Tracker-VPC`.
3. Choose **Route Tables** => Select the Public Route Table => Click **Edit routes** => Add route: `0.0.0.0/0` targeted to `tracker-igw`.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS Console screenshot showing the Internet Gateway attachment and Route Table configuration here.
> 
> ![Internet Gateway Setup](/images/5-Workshop/5.3-S3-vpc/igw-route-setup.png?classes=shadow)
