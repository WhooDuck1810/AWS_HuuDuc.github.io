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

### Bước 5.2.2: Khởi tạo IAM User cho GitHub Actions CI/CD Pipeline

Để tự động hóa quá trình build Docker image và deploy từ GitHub mà không cần nạp tài khoản Root:

1. Truy cập **AWS IAM Console** => **Users** => Chọn **Create user**.
2. Nhập tên user: `github-actions-deployer`.
3. Tại phần **Permissions options**, chọn **Attach policies directly**.
4. Tìm và tích chọn chính sách: `AmazonEC2ContainerRegistryFullAccess`.
5. Xác nhận tạo user, sau đó vào mục **Security credentials** => **Create access key** => Chọn **Command Line Interface (CLI)**.
6. Lưu lại cặp khóa **Access Key ID** và **Secret Access Key** để nạp vào GitHub Secrets.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình chi tiết IAM User `github-actions-deployer` kèm chính sách ECR đã gắn trên AWS Console và dán vào vị trí bên dưới.
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

### Bước 5.2.4: Tại sao cần tạo IAM Account mà KHÔNG dùng Root Account?

- **Nguyên tắc Quyền Tối thiểu (Principle of Least Privilege):** Chỉ cấp đúng quyền cần thiết cho tiến trình tự động hóa.
- **Bảo mật Khóa Root:** Rò rỉ Key của Root có thể làm mất kiểm soát toàn bộ tài khoản và phát sinh chi phí ngoài ý muốn.
- **Khả năng Truy vết (Auditability):** Mọi hành động được AWS CloudTrail ghi nhận rõ danh tính IAM User/Role thực hiện.

