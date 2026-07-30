---
title: "Triển khai Cơ sở dữ liệu Amazon RDS PostgreSQL"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

# 5.3.4 Triển khai Cơ sở dữ liệu Amazon RDS PostgreSQL

Trong bước này, khởi tạo cơ sở dữ liệu quan hệ PostgreSQL quản lý tự động trong Private Subnet.

1. Mở **Amazon RDS Console** => Nhấn **Create database**.
2. Chọn **Standard create** => Engine: **PostgreSQL** (Phiên bản 15.x).
3. Mẫu: **Free Tier** => Cấu hình Instance: `db.t4g.micro`.
4. Đặt tên DB instance: `tracker-maintenance-db`, Master username: `postgres`.
5. Kết nối: Chọn VPC `Tracker-VPC`, chọn Subnet Group trong Private Subnet, Public access: **No**.
6. Security group: Chọn `tracker-rds-sg`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình chi tiết RDS Database `tracker-maintenance-db` và Endpoint kết nối 5432 đính kèm vào bên dưới.
> 
> ![Triển khai RDS Database](/images/5-Workshop/5.3-S3-vpc/rds-db-setup.png?classes=shadow)
