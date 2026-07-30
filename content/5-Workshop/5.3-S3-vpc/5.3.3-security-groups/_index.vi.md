---
title: "Cấu hình Tường lửa ảo (Security Groups)"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3 Cấu hình Tường lửa ảo (Security Groups cho EC2 & RDS)

Trong bước này, tạo 02 tường lửa ảo trong `Tracker-VPC` để phân vùng bảo mật.

1. **EC2 Security Group (`tracker-ec2-sg`):**
   - **Inbound Rules:**
     - SSH (22) => IP cá nhân
     - HTTP (80) => `0.0.0.0/0`
     - HTTPS (443) => `0.0.0.0/0`
     - Custom TCP (3000) => `0.0.0.0/0` (Frontend React)
     - Custom TCP (8081) => `0.0.0.0/0` (Backend Spring Boot API)
2. **RDS Security Group (`tracker-rds-sg`):**
   - **Inbound Rules:**
     - PostgreSQL (5432) => Nguồn: ID của `tracker-ec2-sg`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình Inbound Rules của `tracker-ec2-sg` và `tracker-rds-sg` đính kèm vào bên dưới.
> 
> ![Cấu hình Security Groups](/images/5-Workshop/5.3-S3-vpc/security-groups-setup.png?classes=shadow)
