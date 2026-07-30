---
title: "Khởi tạo Custom VPC & Phân chia Subnet"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1 Khởi tạo Custom VPC & Phân chia Subnet (Public/Private)

Trong bước này, bạn sẽ khởi tạo mạng riêng ảo cách ly cho ứng dụng bảo trì.

1. Truy cập **Amazon VPC Console** => Chọn **Create VPC**.
2. Chọn **VPC only** => Đặt tên Name tag: `Tracker-VPC` => IPv4 CIDR block: `10.1.0.0/16`.
3. Trên danh mục bên trái, chọn **Subnets** => Chọn **Create subnet**:
   - **Public Subnet:** Tên `tracker-public-subnet-1`, CIDR `10.1.1.0/24`, Vùng Availability Zone `ap-southeast-2a`.
   - **Private Subnet:** Tên `tracker-private-subnet-1`, CIDR `10.1.2.0/24`, Vùng Availability Zone `ap-southeast-2b`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình VPC Console hiển thị dải VPC `Tracker-VPC` và danh sách Subnet đính kèm vào bên dưới.
> 
> ![Khởi tạo VPC và Subnet](/images/5-Workshop/5.3-S3-vpc/vpc-subnet-setup.png?classes=shadow)
