---
title: "Khởi tạo Custom VPC & Phân chia Subnet"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1 Khởi tạo Custom VPC & Phân chia Subnet (Public/Private)

Trong bước này, khởi tạo mạng riêng ảo cách ly (`Tracker-VPC-vpc`) và phân chia dải mạng Public Subnet & Private Subnet.

### 1. Thông số khởi tạo VPC
- **Tên VPC (Name tag):** `Tracker-VPC-vpc`
- **Mã VPC ID:** `vpc-0a6e694c7f200c12e`
- **Dải IP (IPv4 CIDR Block):** `10.0.0.0/16`
- **DNS Hostnames:** Enabled (Đã bật)
- **DNS Resolution:** Enabled (Đã bật)

### 2. Phân chia Subnet
- **Public Subnet:** `Tracker-VPC-subnet-public1-ap-southeast-2a` (`10.0.0.0/20`, Vùng AZ: `ap-southeast-2a`)
- **Private Subnet:** `Tracker-VPC-subnet-private1-ap-southeast-2a` (`10.0.128.0/20`, Vùng AZ: `ap-southeast-2a`)

<div style="text-align: center; margin: 20px 0;">

  ![Sơ đồ tài nguyên VPC](/images/5-Workshop/5.3-S3-vpc/5.3.1-vpc-subnets/vpc-resource-map.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.3.1. Sơ đồ tài nguyên (Resource Map) của Tracker-VPC-vpc hiển thị liên kết giữa VPC, Subnet, Bảng tuyến đường và Internet Gateway.</div>
</div>

<div style="text-align: center; margin: 20px 0;">

  ![Danh sách Subnet](/images/5-Workshop/5.3-S3-vpc/5.3.1-vpc-subnets/subnets-list.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.3.2. Danh sách Subnet công khai và riêng tư được khởi tạo trong Tracker-VPC-vpc.</div>
</div>
