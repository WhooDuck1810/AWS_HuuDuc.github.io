---
title: "Quản lý Log & Giám sát tập trung với CloudWatch"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

# 5.6.2 Quản lý Log & Giám sát tập trung với CloudWatch

Trong bước này, theo dõi chỉ số hiệu năng cơ sở dữ liệu thời gian thực và quản lý các nhóm nhật ký (Log Groups) trên Amazon CloudWatch.

### 1. Giám sát Database Insights
- **Amazon CloudWatch Database Insights** tự động ghi nhận và trực quan hóa tải CPU, số lượng phiên làm việc active và danh sách các câu lệnh SQL chiếm nhiều tài nguyên nhất của `tracker-maintenance-db`.
- Tải cơ sở dữ liệu (Database Load Utilization): `0.1%` (Hệ thống vận hành ổn định).

<div style="text-align: center; margin: 20px 0;">

  ![CloudWatch Database Insights](/images/5-Workshop/5.6-Cleanup/5.6.2-cloudwatch-logs/cloudwatch-db-insights.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.6.3. Giao diện CloudWatch Database Insights giám sát chỉ số tải CPU, active sessions và Top câu lệnh SQL của database.</div>
</div>

### 2. Quản lý Nhóm log (Log Groups)
- **`RDSOSMetrics`**: Nhóm log tự động ghi nhận các chỉ số hệ điều hành của máy chủ RDS PostgreSQL (Lưu trữ 1 tháng).
- **Log Container ứng dụng**: Được đẩy trực tiếp từ các Docker container về CloudWatch Log Groups.

<div style="text-align: center; margin: 20px 0;">

  ![CloudWatch Log Groups](/images/5-Workshop/5.6-Cleanup/5.6.2-cloudwatch-logs/cloudwatch-log-groups.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.6.4. Trang quản lý CloudWatch Log Groups hiển thị các nhóm log hạ tầng bao gồm RDSOSMetrics.</div>
</div>
