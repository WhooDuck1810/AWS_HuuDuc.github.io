---
title: "Dọn dẹp tài nguyên"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. Dọn dẹp tài nguyên

Khi hoàn thành bài lab hoặc khi không còn nhu cầu sử dụng hệ thống, hãy thực hiện các bước sau để tránh phát sinh chi phí AWS ngoài ý muốn:

---

### Bước 5.5.1: Tạm dừng máy chủ EC2 (Stop Instance)

Nếu muốn giữ lại cấu hình để sử dụng cho lần sau:
1. Truy cập **EC2 Console** => **Instances**.
2. Chọn máy chủ `tracker-ec2-server` => Chọn **Instance State** => **Stop instance**.
3. Máy chủ EC2 khi ở trạng thái Stop sẽ **không phát sinh chi phí compute**, chỉ tính phí lưu trữ ổ cứng EBS (~$0.08/GB/tháng).

---

### Bước 5.5.2: Tạm dừng cơ sở dữ liệu RDS

1. Truy cập **RDS Console** => **Databases**.
2. Chọn database `tracker-maintenance-db` => Chọn **Actions** => **Stop temporarily**.
3. RDS cho phép tạm dừng tối đa 7 ngày. Trong thời gian tạm dừng, bạn chỉ trả phí lưu trữ dữ liệu.

---

### Bước 5.5.3: Xóa tài nguyên trên S3 (Tùy chọn)

1. Truy cập **S3 Console** => `tracker-maintenance-images-123`.
2. Chọn toàn bộ file ảnh => Chọn **Delete**.
3. Bucket rỗng sẽ không phát sinh chi phí lưu trữ.

---

### Bước 5.5.4: Xóa Docker Image trên ECR (Tùy chọn)

1. Truy cập **ECR Console** => **Private Repositories**.
2. Chọn `tracker-be` và `tracker-fe` => Xóa các tag image `:latest`.

---

### Bước 5.5.5: Xóa Log Groups trên CloudWatch (Tùy chọn)

1. Truy cập **CloudWatch Console** => **Log Groups**.
2. Chọn `/tracker-maintenance/backend` và `/tracker-maintenance/frontend` => **Actions** => **Delete log group**.

---

### Bước 5.5.6: Xóa hoàn toàn tài nguyên (Khi kết thúc thực tập)

Nếu muốn hủy bỏ hoàn toàn môi trường:
1. **Terminate** (Xóa vĩnh viễn) máy chủ EC2.
2. **Delete** cơ sở dữ liệu RDS PostgreSQL.
3. **Delete** S3 Bucket (xóa hết dữ liệu bên trong trước khi xóa bucket).
4. **Delete** các ECR Repositories.
5. **Delete** chứng chỉ SSL ACM tại `us-east-1`.
6. **Delete** Route 53 Hosted Zone.
7. **Revoke** Access Key của IAM User & Xóa IAM Role.
