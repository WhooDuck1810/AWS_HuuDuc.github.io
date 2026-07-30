---
title: "Triển khai Cơ sở dữ liệu Amazon RDS PostgreSQL"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

# 5.3.4 Triển khai Cơ sở dữ liệu Amazon RDS PostgreSQL

Trong bước này, khởi tạo cơ sở dữ liệu quan hệ PostgreSQL được quản lý tự động trong Private Subnet của `Tracker-VPC`.

### Thông số kỹ thuật Database
- **Tên DB Identifier:** `tracker-maintenance-db`
- **Engine:** PostgreSQL 15.x
- **Instance Class:** `db.t3.micro`
- **Trạng thái:** **Available** (Hoạt động) ✅
- **Vùng (Region & AZ):** `ap-southeast-2b` (Sydney)
- **Truy cập công khai:** Disabled (Chỉ truy cập nội bộ từ EC2 trong VPC)

<div style="text-align: center; margin: 20px 0;">

  ![RDS Database Summary](/images/5-Workshop/5.3-S3-vpc/5.3.4-rds-database/rds-summary.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.3.5. Tổng quan cơ sở dữ liệu Amazon RDS PostgreSQL trên AWS Console.</div>
</div>
