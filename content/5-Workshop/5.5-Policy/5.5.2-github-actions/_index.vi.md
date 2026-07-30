---
title: "Cấu hình GitHub Secrets & Luồng Tự động hóa CI/CD"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2 Cấu hình GitHub Secrets & Luồng Tự động hóa CI/CD

Trong bước này, lưu trữ các thông tin xác thực an toàn trong GitHub Repository Secrets và tự động hóa luồng triển khai lên máy chủ AWS EC2.

### 1. Cấu hình Repository Secrets
Trong GitHub Repository => Vào **Settings** => **Secrets and variables** => **Actions**, khai báo:
- `AWS_ACCESS_KEY_ID`: Access key của IAM user `tracker-s3-uploader-2`
- `AWS_SECRET_ACCESS_KEY`: Secret access key của IAM user `tracker-s3-uploader-2`
- `EC2_HOST`: Elastic IP của máy chủ EC2 (`3.106.194.112`)
- `EC2_SSH_KEY`: SSH Private Key đăng nhập máy chủ `ec2-user`

### 2. Nhật ký chạy luồng tự động (Workflow Runs)
Mỗi thao tác `git push` mã nguồn lên nhánh `main` sẽ tự động kích hoạt tiến trình đóng gói Docker container, đẩy image về ECR và SSH vào EC2 để triển khai ứng dụng.

<div style="text-align: center; margin: 20px 0;">

  ![Lịch sử chạy GitHub Actions](/images/5-Workshop/5.5-Policy/5.5.2-github-actions/github-actions-runs.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.5.2. Nhật ký chạy thành công các luồng tự động hóa Deploy to EC2 trên GitHub Actions.</div>
</div>
