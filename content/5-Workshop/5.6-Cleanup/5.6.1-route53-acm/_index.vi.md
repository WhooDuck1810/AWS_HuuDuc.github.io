---
title: "Cấu hình Tên miền Route 53 & Chứng chỉ ACM SSL"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1 Cấu hình Tên miền Route 53 & Chứng chỉ ACM SSL

Trong bước này, kết nối tên miền mua từ nhà cung cấp bên ngoài (`trackermaint.dpdns.org`) vào AWS Route 53 và đăng ký cấp chứng chỉ SSL/TLS miễn phí qua AWS Certificate Manager (ACM).

### 1. Đăng ký Tên miền ngoài & Khởi tạo Route 53 Hosted Zone
- Tên miền cá nhân `trackermaint.dpdns.org` được mua từ nhà cung cấp bên ngoài.
- Khởi tạo 01 **Public Hosted Zone** trên **AWS Route 53**, sau đó trỏ bản tin NS (Name Server) từ nhà cung cấp tên miền về các máy chủ DNS của AWS.
- Tạo bản ghi **A Record** (Alias) định tuyến lưu lượng truy cập tới máy chủ ứng dụng.

<div style="text-align: center; margin: 20px 0;">

  ![Route 53 Records](/images/5-Workshop/5.6-Cleanup/5.6.1-route53-acm/route53-records.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.6.1. Danh sách bản ghi Route 53 Hosted Zone cho tên miền trackermaint.dpdns.org bao gồm A Record, NS, SOA và CNAME xác minh.</div>
</div>

### 2. Đăng ký Chứng chỉ SSL trên AWS Certificate Manager (ACM)
- Yêu cầu cấp chứng chỉ bảo mật SSL/TLS công khai cho tên miền `trackermaint.dpdns.org` trên **AWS Certificate Manager (ACM)**.
- Thực hiện xác minh quyền sở hữu tên miền tự động thông qua bản ghi CNAME trên Route 53.
- **Trạng thái chứng chỉ:** **Issued** (Đã cấp thành công và đang được sử dụng) ✅

<div style="text-align: center; margin: 20px 0;">

  ![ACM Certificate Status](/images/5-Workshop/5.6-Cleanup/5.6.1-route53-acm/acm-certificate.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.6.2. Trang quản lý chứng chỉ ACM hiển thị trạng thái Issued cho tên miền trackermaint.dpdns.org.</div>
</div>
