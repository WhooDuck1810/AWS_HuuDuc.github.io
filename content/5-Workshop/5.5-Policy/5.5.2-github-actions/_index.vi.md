---
title: "Cấu hình GitHub Secrets & Luồng Tự động hóa CI/CD"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2 Cấu hình GitHub Secrets & Luồng Tự động hóa CI/CD

Trong bước này, nạp các biến bí mật và thiết lập luồng triển khai tự động hóa.

1. Trong GitHub Repository => Vào **Settings** => **Secrets and variables** => **Actions**.
2. Thêm các Secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `EC2_HOST`, `EC2_SSH_KEY`.
3. Push file `.github/workflows/deploy.yml` lên nhánh `main` để kích hoạt luồng tự động build Docker, push ECR và deploy EC2.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình GitHub Actions tích xanh chạy thành công luồng `Deploy to EC2` đính kèm vào bên dưới.
> 
> ![Cấu hình GitHub Actions](/images/5-Workshop/5.3-S3-vpc/github-actions-setup.png?classes=shadow)
