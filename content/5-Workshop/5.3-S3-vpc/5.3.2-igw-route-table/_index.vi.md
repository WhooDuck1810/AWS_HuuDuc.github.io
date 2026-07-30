---
title: "Cấu hình Internet Gateway & Bảng tuyến đường"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2 Cấu hình Internet Gateway & Bảng tuyến đường (Route Tables)

Trong bước này, mở kết nối Internet cho dải Public Subnet.

1. Trên VPC Console, chọn **Internet Gateways** => Chọn **Create internet gateway** => Đặt tên: `tracker-igw`.
2. Chọn `tracker-igw` => Nhấn **Actions** => Chọn **Attach to VPC** => Chọn `Tracker-VPC`.
3. Chọn **Route Tables** => Chọn Public Route Table => Nhấn **Edit routes** => Thêm tuyến đường `0.0.0.0/0` trỏ tới `tracker-igw`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình Internet Gateway đã Attach vào VPC và Bảng tuyến đường Route Table công khai đính kèm vào bên dưới.
> 
> ![Cấu hình Internet Gateway](/images/5-Workshop/5.3-S3-vpc/igw-route-setup.png?classes=shadow)
