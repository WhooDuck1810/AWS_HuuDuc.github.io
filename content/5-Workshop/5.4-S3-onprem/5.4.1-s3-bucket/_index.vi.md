---
title: "Cấu hình Amazon S3 Bucket lưu trữ ảnh nghiệm thu"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1 Cấu hình Amazon S3 Bucket lưu trữ ảnh nghiệm thu

Trong bước này, tạo Amazon S3 Bucket để lưu trữ ảnh thiết bị và bằng chứng bảo trì tải lên từ phía ứng dụng.

1. Mở **Amazon S3 Console** => Nhấn **Create bucket**.
2. Đặt tên Bucket: `tracker-maintenance-images-123`, Vùng: `ap-southeast-2` (Sydney).
3. Cấu hình **Block Public Access:** Bỏ tích *Block all public access* (Disable).
4. Lưu tạo bucket, sau đó mở tab **Permissions** => Thêm **Bucket Policy**:
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
5. Cấu hình quy tắc **CORS** cho phép các phương thức `PUT` và `GET` từ tên miền ứng dụng Web.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình tổng quan S3 Bucket và trạng thái Block Public Access đính kèm vào bên dưới.
> 
> ![Cấu hình S3 Bucket](/images/5-Workshop/5.3-S3-vpc/s3-bucket-setup.png?classes=shadow)
