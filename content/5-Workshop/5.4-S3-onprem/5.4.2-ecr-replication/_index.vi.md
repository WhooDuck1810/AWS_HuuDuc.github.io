---
title: "Cấu hình Amazon ECR & Cross-Region Replication"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2 Cấu hình Amazon ECR & Cross-Region Replication (Singapore & Ohio)

Trong bước này, tạo kho lưu trữ Docker Registry riêng tư trên Amazon ECR cho các image backend (`tracker-be`) và frontend (`tracker-fe`).

> [!NOTE]
> ℹ️ **Lưu ý quan trọng về danh sách Repository:** Trong hình ảnh bên dưới, repository `aws/tracker_maintenance_app` là kho lưu trữ thử nghiệm ban đầu. Bạn vui lòng tập trung vào 02 repository chính thức phục vụ triển khai hệ thống là: **`tracker-be`** và **`tracker-fe`**.

<div style="text-align: center; margin: 20px 0;">

  ![ECR Repositories](/images/5-Workshop/5.4-S3-onprem/5.4.2-ecr-replication/ecr-repositories.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.4.2. Danh sách Amazon ECR Private Repositories bao gồm tracker-be và tracker-fe.</div>
</div>
