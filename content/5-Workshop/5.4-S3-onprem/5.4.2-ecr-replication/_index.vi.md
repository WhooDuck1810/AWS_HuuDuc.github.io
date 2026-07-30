---
title: "Cấu hình Amazon ECR & Cross-Region Replication"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2 Cấu hình Amazon ECR & Cross-Region Replication (Singapore & Ohio)

Trong bước này, tạo kho Docker Registry riêng tư và thiết lập tự động đồng bộ sang vùng Singapore và Ohio.

1. Mở **Amazon ECR Console** => Tạo 2 repository riêng tư: `tracker-be` và `tracker-fe`.
2. Mở **Private registry** => **Replication configuration** => Chọn **Add rule**.
3. Vùng đích: Thêm `ap-southeast-1` (Singapore) và `us-east-2` (Ohio).

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình danh sách ECR Repository và quy tắc Cross-Region Replication đính kèm vào bên dưới.
> 
> ![Cấu hình ECR Replication](/images/5-Workshop/5.3-S3-vpc/ecr-replication-setup.png?classes=shadow)
