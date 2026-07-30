---
title: "Cấu hình Tên miền Route 53 & Chứng chỉ ACM SSL"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1 Cấu hình Tên miền Route 53 & Chứng chỉ ACM SSL

Trong bước này, trỏ tên miền cá nhân về IP máy chủ EC2 và khởi tạo chứng chỉ SSL.

1. Mở **Route 53 Console** => Tạo Hosted Zone cho `trackermaint.dpdns.org`.
2. Tạo **A Record** trỏ tên miền `trackermaint.dpdns.org` về Elastic IP của EC2 (`3.106.194.112`).
3. Khởi tạo chứng chỉ **ACM Certificate** tại vùng `us-east-1` (N. Virginia) cho `trackermaint.dpdns.org` để chuẩn bị cho CloudFront.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình Route 53 A Record trỏ IP đính kèm vào bên dưới.
> 
> ![Cấu hình Route 53](/images/5-Workshop/5.3-S3-vpc/route53-acm-setup.png?classes=shadow)
