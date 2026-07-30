---
title: "Chuẩn bị"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. Chuẩn bị

Trước khi bắt đầu triển khai dự án **Tracker Maintenance System** trên AWS, cần chuẩn bị môi trường máy tính cá nhân và các quyền truy cập IAM an toàn.

---

### Bước 5.2.1: Chuẩn bị Môi trường Máy cá nhân (Local Workstation)

1. Cài đặt **JDK 21 (Amazon Corretto hoặc Temurin)** và **Node.js v20 LTS** trên máy cá nhân.
2. Cài đặt **Docker Desktop & Git** để đóng gói và kiểm thử container cục bộ.
3. Chuẩn bị file `.env` cá nhân và đảm bảo các thông tin nhạy cảm (`AWS_ACCESS_KEY_ID`, `JWT_SECRET`, mật khẩu database) đã được khai báo trong `.gitignore`.

---

### Bước 5.2.2: Khởi tạo IAM User (`tracker-s3-uploader-2`) cho Upload S3 & Triển khai

Để cấp quyền cho Backend Spring Boot tải ảnh nghiệm thu lên S3 và hỗ trợ tự động hóa triển khai mà không cần dùng tài khoản Root hoặc `admin1`:

1. Truy cập **AWS IAM Console** => **Users** => Chọn **Create user**.
2. Nhập tên user: `tracker-s3-uploader-2`.
3. Tại phần **Permissions options**, chọn **Attach policies directly**.
4. Tìm và tích chọn chính sách quyền lưu trữ S3 (ví dụ `AmazonS3FullAccess` hoặc chính sách S3 tùy chỉnh) và quyền ECR (`AmazonEC2ContainerRegistryFullAccess`).
5. Xác nhận tạo user, sau đó vào mục **Security credentials** => **Create access key** => Chọn **Command Line Interface (CLI)**.
6. Lưu lại cặp khóa **Access Key ID** và **Secret Access Key** để nạp vào `.env` backend và GitHub Secrets.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình chi tiết IAM User `tracker-s3-uploader-2` kèm chính sách đã gắn trên AWS Console và dán vào vị trí bên dưới.
> 
> ![Khởi tạo IAM User](/images/5-Workshop/5.2-Prerequisite/iam-user-setup.png?classes=shadow)

---

### Bước 5.2.3: Khởi tạo IAM Role cho Máy chủ EC2

Cấp quyền cho máy chủ EC2 kéo Docker Image từ ECR và đẩy log về CloudWatch mà không cần lưu khóa truy cập cố định trên máy chủ:

1. Truy cập **IAM Console** => **Roles** => Chọn **Create role**.
2. Chọn Trusted entity type: **AWS Service**, trường hợp sử dụng: **EC2**.
3. Gắn 02 chính sách quyền chuẩn của AWS:
   - `AmazonEC2ContainerRegistryReadOnly`
   - `CloudWatchAgentServerPolicy`
4. Đặt tên Role: `tracker-ec2-role` và nhấn **Create role**.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình IAM Role `tracker-ec2-role` hiển thị 2 chính sách quyền đã gắn trên AWS Console và dán vào vị trí bên dưới.
> 
> ![Khởi tạo IAM Role](/images/5-Workshop/5.2-Prerequisite/iam-role-setup.png?classes=shadow)

---

### Bước 5.2.4: Tại sao cần dùng IAM User (`tracker-s3-uploader-2`) thay vì Root Account hoặc `admin1`?

- **Nguyên tắc Quyền Tối thiểu (Principle of Least Privilege):** Chỉ cấp đúng quyền upload S3/ECR cho ứng dụng qua `tracker-s3-uploader-2` thay vì dùng tài khoản có toàn quyền quản trị (`admin1` / Root).
- **Cách ly Rủi ro Bảo mật:** Nếu Access Key bị lộ, hacker chỉ có thể thao tác trên S3 bucket/ECR mà không thể can thiệp hoặc xóa các tài nguyên AWS khác.
- **Khả năng Truy vết (Auditability):** Mọi hành động gọi API S3/ECR đều được AWS CloudTrail ghi lại rõ danh tính `tracker-s3-uploader-2`.
