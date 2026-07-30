---
title: "Configure Internet Gateway & Route Tables"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2 Configure Internet Gateway & Route Tables

In this step, verify that the Internet Gateway (`Tracker-VPC-igw`) is attached to `Tracker-VPC-vpc` and routed through the Public Route Table.

### 1. Internet Gateway Details
- **IGW Name:** `Tracker-VPC-igw`
- **IGW ID:** `igw-0f51a5a491c36f5ae`
- **State:** **Attached** ✅
- **Attached VPC:** `vpc-0a6e694c7f200c12e | Tracker-VPC-vpc`

### 2. Route Table Routing
- **Public Route Table:** `Tracker-VPC-rtb-public`
- **Destination:** `0.0.0.0/0`
- **Target:** `igw-0f51a5a491c36f5ae` (`Tracker-VPC-igw`)

<div style="text-align: center; margin: 20px 0;">

  ![Internet Gateway Setup](/images/5-Workshop/5.3-S3-vpc/5.3.2-igw-route-table/igw-setup.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.3.3. Internet Gateway (Tracker-VPC-igw) details showing Attached state to Tracker-VPC-vpc.</div>
</div>
