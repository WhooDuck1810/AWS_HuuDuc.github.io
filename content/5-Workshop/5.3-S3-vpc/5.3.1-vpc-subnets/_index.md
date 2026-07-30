---
title: "Create Custom VPC & Subnets"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1 Create Custom VPC & Subnets

In this step, provision the isolated virtual private cloud (`Tracker-VPC-vpc`) and allocate Public & Private subnets.

### 1. VPC Configuration Parameters
- **VPC Name:** `Tracker-VPC-vpc`
- **VPC ID:** `vpc-0a6e694c7f200c12e`
- **IPv4 CIDR Block:** `10.0.0.0/16`
- **DNS Hostnames:** Enabled
- **DNS Resolution:** Enabled

### 2. Subnet Allocation
- **Public Subnet:** `Tracker-VPC-subnet-public1-ap-southeast-2a` (`10.0.0.0/20`, AZ: `ap-southeast-2a`)
- **Private Subnet:** `Tracker-VPC-subnet-private1-ap-southeast-2a` (`10.0.128.0/20`, AZ: `ap-southeast-2a`)

<div style="text-align: center; margin: 20px 0;">

  ![Tracker VPC Resource Map](/images/5-Workshop/5.3-S3-vpc/5.3.1-vpc-subnets/vpc-resource-map.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.3.1. AWS VPC Resource Map showing Tracker-VPC-vpc, Subnets, Route Tables, and Internet Gateway connection.</div>
</div>

<div style="text-align: center; margin: 20px 0;">

  ![Tracker Subnets List](/images/5-Workshop/5.3-S3-vpc/5.3.1-vpc-subnets/subnets-list.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.3.2. Subnet allocation list showing public and private subnets inside Tracker-VPC-vpc.</div>
</div>
