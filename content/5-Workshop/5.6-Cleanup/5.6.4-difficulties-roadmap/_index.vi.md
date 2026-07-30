---
title: "Khó khăn gặp phải & Hướng phát triển tương lai"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.6.4. </b> "
---

# 5.6.4 Khó khăn gặp phải & Hướng phát triển tương lai

### 1. Khó khăn gặp phải & Giải pháp
- **Đường dẫn Windows vs Linux:** Xử lý bằng cách chuyển luồng build React Router sang Linux runner trên GitHub Actions.
- **Giới hạn RAM EC2:** Giới hạn bộ nhớ JVM `-Xms256m -Xmx512m` trong Docker Compose.
- **Xác minh CloudFront:** Chứng chỉ ACM đã sẵn sàng, chờ AWS Support mở khóa.

### 2. Hướng phát triển tương lai
- **Tự động hóa Hạ tầng (AWS CDK v2):** Đóng gói 100% tài nguyên AWS thành mã nguồn TypeScript.
- **Kiến trúc Đa vùng Active-Active:** Mở rộng máy chủ EC2 sang Singapore và Ohio tận dụng ECR Replication.
- **Aurora Global Database:** Chuyển đổi sang Aurora PostgreSQL để đồng bộ dữ liệu toàn cầu.
- **Tự động mở rộng (ASG + ALB):** Đặt nhóm máy chủ EC2 phía sau Application Load Balancer.
