---
title: "Cấu hình Tường lửa ảo (Security Groups)"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3 Cấu hình Tường lửa ảo (Security Groups cho EC2 & RDS)

Trong bước này, tạo 02 tường lửa ảo (Security Groups) để phân vùng bảo mật giữa ứng dụng Web công khai và cơ sở dữ liệu riêng tư.

### 1. Cấu hình Inbound Rules (Luồng vào)
- **SSH (22):** `0.0.0.0/0` (Truy cập từ xa)
- **HTTP (80):** `0.0.0.0/0` (Lưu lượng Web thường)
- **HTTPS (443):** `0.0.0.0/0` (Lưu lượng Web bảo mật SSL)
- **Custom TCP (3000):** Container Frontend React
- **Custom TCP (8081):** Container Backend Spring Boot API

### 2. Cấu hình Outbound Rules (Luồng ra)
- **All Traffic:** `0.0.0.0/0`
- **PostgreSQL (5432):** Kết nối trực tiếp tới Security Group của RDS (`ec2-rds-1` / `sg-0c0bf5f62fdf25148`)

<div style="text-align: center; margin: 20px 0;">

  ![Security Groups Rules](/images/5-Workshop/5.3-S3-vpc/5.3.3-security-groups/security-groups-rules.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.3.4. Chi tiết Inbound và Outbound Rules của Security Group trên AWS Console.</div>
</div>
