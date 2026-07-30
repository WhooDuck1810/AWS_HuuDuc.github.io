---
title: "Cấu hình Internet Gateway & Bảng tuyến đường"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2 Cấu hình Internet Gateway & Bảng tuyến đường (Route Tables)

Trong bước này, xác nhận Cổng Internet (Internet Gateway `Tracker-VPC-igw`) đã được kết nối (Attached) tới `Tracker-VPC-vpc` và định tuyến ra ngoài Internet qua Bảng tuyến đường công khai.

### 1. Thông số Internet Gateway
- **Tên IGW:** `Tracker-VPC-igw`
- **Mã IGW ID:** `igw-0f51a5a491c36f5ae`
- **Trạng thái (State):** **Attached** (Đã kết nối) ✅
- **VPC kết nối:** `vpc-0a6e694c7f200c12e | Tracker-VPC-vpc`

### 2. Định tuyến Route Table
- **Public Route Table:** `Tracker-VPC-rtb-public`
- **Đích đến (Destination):** `0.0.0.0/0`
- **Mục tiêu (Target):** `igw-0f51a5a491c36f5ae` (`Tracker-VPC-igw`)

<div style="text-align: center; margin: 20px 0;">

  ![Cấu hình Internet Gateway](/images/5-Workshop/5.3-S3-vpc/5.3.2-igw-route-table/igw-setup.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.3.3. Chi tiết Internet Gateway (Tracker-VPC-igw) trạng thái Attached thành công vào Tracker-VPC-vpc.</div>
</div>
