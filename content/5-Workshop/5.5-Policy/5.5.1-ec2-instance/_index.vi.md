---
title: "Khởi tạo Máy chủ EC2 & Elastic IP"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1 Khởi tạo Máy chủ EC2 & Elastic IP

Trong bước này, khởi tạo máy chủ ảo EC2 và gắn địa chỉ Elastic IP tĩnh.

1. Mở **Amazon EC2 Console** => Chọn **Launch instance**.
2. Đặt tên: `tracker-ec2-server`, AMI: **Amazon Linux 2023**, Loại: `t2.micro`.
3. Mạng: VPC `Tracker-VPC`, Subnet `tracker-public-subnet-1`, Security group `tracker-ec2-sg`.
4. Nâng cao: IAM instance profile => Chọn `tracker-ec2-role`.
5. Tạo 01 **Elastic IP** và Associate trực tiếp vào máy chủ EC2 (`3.106.194.112`).
6. SSH vào EC2 và cài đặt Docker & Docker Compose:
   ```bash
   sudo yum install docker -y
   sudo systemctl enable docker && sudo systemctl start docker
   sudo usermod -aG docker ec2-user
   ```

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình EC2 Instance `tracker-ec2-server` cùng Elastic IP đã gắn đính kèm vào bên dưới.
> 
> ![Khởi tạo EC2 Instance](/images/5-Workshop/5.3-S3-vpc/ec2-instance-setup.png?classes=shadow)
