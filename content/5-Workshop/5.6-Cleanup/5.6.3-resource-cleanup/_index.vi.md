---
title: "Hướng dẫn Dọn dẹp Tài nguyên"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

# 5.6.3 Hướng dẫn Dọn dẹp Tài nguyên (Stop vs Terminate)

Thực hiện các bước dọn dẹp để tạm dừng hoặc xóa hẳn tài nguyên AWS để tránh phát sinh chi phí.

- **Stop EC2:** Giữ nguyên cấu hình, không tốn tiền compute.
- **Stop RDS:** Tạm dừng database trong vòng 7 ngày.
- **Xóa S3 & ECR Objects:** Xóa các file ảnh nghiệm thu và image tag cũ.
- **Xóa hoàn toàn:** Terminate EC2, RDS, S3, ECR, Route 53, ACM và IAM Role.
