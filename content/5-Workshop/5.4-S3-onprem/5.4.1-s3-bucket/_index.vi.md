---
title: "Cấu hình Amazon S3 Bucket lưu trữ ảnh nghiệm thu"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1 Cấu hình Amazon S3 Bucket lưu trữ ảnh nghiệm thu

Trong bước này, tạo Amazon S3 Bucket để lưu trữ ảnh thiết bị và bằng chứng bảo trì tải lên từ phía ứng dụng.

### Cấu hình Bucket Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::tracker-maintenance-images-123/*"
    }
  ]
}
```

<div style="text-align: center; margin: 20px 0;">

  ![S3 Bucket Policy](/images/5-Workshop/5.4-S3-onprem/5.4.1-s3-bucket/s3-policy.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Hình 5.4.1. Cấu hình quyền S3 Bucket bao gồm Block Public Access Off và chính sách đọc công khai Bucket Policy.</div>
</div>
