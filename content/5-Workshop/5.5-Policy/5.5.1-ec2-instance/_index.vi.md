---
title: "Khởi tạo Máy chủ EC2 & Elastic IP"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1 Khởi tạo Máy chủ EC2 & Elastic IP

Trong bước này, khởi tạo máy chủ ảo Amazon EC2 (`tracker-maintenance-server`) và gán địa chỉ Elastic IP tĩnh (`3.106.194.112`).

### Thông số tổng quan Máy chủ EC2
- **Mã Instance ID:** `i-00df249a6bd8ee5f4`
- **Tên Máy chủ:** `tracker-maintenance-server`
- **Trạng thái (State):** **Running** (Đang chạy) ✅
- **Cấu hình Instance Type:** `t3.small`
- **Địa chỉ Public IPv4:** `3.106.194.112` (Elastic IP)
- **Địa chỉ Private IPv4:** `172.31.39.246`
- **IAM Role đính kèm:** `ec2-ecr-role`

<div style="text-align: center; margin: 20px 0;">

  ![EC2 Instance Summary](/images/5-Workshop/5.5-Policy/5.5.1-ec2-instance/ec2-summary.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.5.1. Chi tiết máy chủ EC2 (tracker-maintenance-server) trạng thái Running kèm Elastic IP trên AWS Console.</div>
</div>
