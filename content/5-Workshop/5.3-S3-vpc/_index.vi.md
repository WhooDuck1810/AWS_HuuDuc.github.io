---
title: "Các bước thực hiện"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3. Các bước thực hiện

Thực hiện theo các bước hướng dẫn dưới đây để khởi tạo hạ tầng Cloud, đóng gói container và tự động hóa triển khai ứng dụng **Tracker Maintenance System** trên AWS.

---

### Bước 5.3.1: Khởi tạo Custom VPC & Phân chia Subnet

1. Truy cập **Amazon VPC Console** $\rightarrow$ Chọn **Create VPC**.
2. Chọn **VPC only** $\rightarrow$ Đặt tên Name tag: `Tracker-VPC` $\rightarrow$ IPv4 CIDR block: `10.1.0.0/16`.
3. Trên danh mục bên trái, chọn **Subnets** $\rightarrow$ Chọn **Create subnet**:
   - **Public Subnet:** Tên `tracker-public-subnet-1`, CIDR `10.1.1.0/24`, Vùng Availability Zone `ap-southeast-2a`.
   - **Private Subnet:** Tên `tracker-private-subnet-1`, CIDR `10.1.2.0/24`, Vùng Availability Zone `ap-southeast-2b`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình VPC Console hiển thị dải VPC `Tracker-VPC` và danh sách Subnet đính kèm vào bên dưới.
> 
> ![Khởi tạo VPC và Subnet](/images/5-Workshop/5.3-S3-vpc/vpc-subnet-setup.png?classes=shadow)

---

### Bước 5.3.2: Cấu hình Internet Gateway & Bảng tuyến đường (Route Tables)

1. Trên VPC Console, chọn **Internet Gateways** $\rightarrow$ เลือก **Create internet gateway** $\rightarrow$ Đặt tên: `tracker-igw`.
2. Chọn `tracker-igw` $\rightarrow$ Nhấn **Actions** $\rightarrow$ Chọn **Attach to VPC** $\rightarrow$ Chọn `Tracker-VPC`.
3. Chọn **Route Tables** $\rightarrow$ Chọn Public Route Table $\rightarrow$ Nhấn **Edit routes** $\rightarrow$ Thêm tuyến đường `0.0.0.0/0` trỏ tới `tracker-igw`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình Internet Gateway đã Attach vào VPC và Bảng tuyến đường Route Table công khai đính kèm vào bên dưới.
> 
> ![Cấu hình Internet Gateway](/images/5-Workshop/5.3-S3-vpc/igw-route-setup.png?classes=shadow)

---

### Bước 5.3.3: Cấu hình Tường lửa ảo (Security Groups)

Tạo 02 Security Groups trong `Tracker-VPC` để phân vùng bảo mật:

1. **EC2 Security Group (`tracker-ec2-sg`):**
   - **Inbound Rules:**
     - SSH (22) $\rightarrow$ IP cá nhân
     - HTTP (80) $\rightarrow$ `0.0.0.0/0`
     - HTTPS (443) $\rightarrow$ `0.0.0.0/0`
     - Custom TCP (3000) $\rightarrow$ `0.0.0.0/0` (Frontend React)
     - Custom TCP (8081) $\rightarrow$ `0.0.0.0/0` (Backend Spring Boot API)
2. **RDS Security Group (`tracker-rds-sg`):**
   - **Inbound Rules:**
     - PostgreSQL (5432) $\rightarrow$ Nguồn: ID của `tracker-ec2-sg`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình Inbound Rules của `tracker-ec2-sg` và `tracker-rds-sg` đính kèm vào bên dưới.
> 
> ![Cấu hình Security Groups](/images/5-Workshop/5.3-S3-vpc/security-groups-setup.png?classes=shadow)

---

### Bước 5.3.4: Triển khai Cơ sở dữ liệu Amazon RDS PostgreSQL

1. Mở **Amazon RDS Console** $\rightarrow$ Nhấn **Create database**.
2. Chọn **Standard create** $\rightarrow$ Engine: **PostgreSQL** (Phiên bản 15.x).
3. Mẫu: **Free Tier** $\rightarrow$ Cấu hình Instance: `db.t4g.micro`.
4. Đặt tên DB instance: `tracker-maintenance-db`, Master username: `postgres`.
5. Kết nối: Chọn VPC `Tracker-VPC`, chọn Subnet Group trong Private Subnet, Public access: **No**.
6. Security group: Chọn `tracker-rds-sg`.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình chi tiết RDS Database `tracker-maintenance-db` và Endpoint kết nối 5432 đính kèm vào bên dưới.
> 
> ![Triển khai RDS Database](/images/5-Workshop/5.3-S3-vpc/rds-db-setup.png?classes=shadow)

---

### Bước 5.3.5: Cấu hình Amazon S3 Bucket lưu trữ Ảnh

1. Mở **Amazon S3 Console** $\rightarrow$ Nhấn **Create bucket**.
2. Đặt tên Bucket: `tracker-maintenance-images-123`, Vùng: `ap-southeast-2` (Sydney).
3. Cấu hình **Block Public Access:** Bỏ tích *Block all public access* (Disable).
4. Lưu tạo bucket, sau đó mở tab **Permissions** $\rightarrow$ Thêm **Bucket Policy**:
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

---

### Bước 5.3.6: Cấu hình Amazon ECR & Sao chép đa vùng

1. Mở **Amazon ECR Console** $\rightarrow$ Tạo 2 repository riêng tư: `tracker-be` và `tracker-fe`.
2. Mở **Private registry** $\rightarrow$ **Replication configuration** $\rightarrow$ Chọn **Add rule**.
3. Vùng đích: Thêm `ap-southeast-1` (Singapore) và `us-east-2` (Ohio).

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình danh sách ECR Repository và quy tắc Cross-Region Replication đính kèm vào bên dưới.
> 
> ![Cấu hình ECR Replication](/images/5-Workshop/5.3-S3-vpc/ecr-replication-setup.png?classes=shadow)

---

### Bước 5.3.7: Khởi tạo Máy chủ Amazon EC2 & Gán Elastic IP

1. Mở **Amazon EC2 Console** $\rightarrow$ Chọn **Launch instance**.
2. Đặt tên: `tracker-ec2-server`, AMI: **Amazon Linux 2023**, Loại: `t2.micro`.
3. Mạng: VPC `Tracker-VPC`, Subnet `tracker-public-subnet-1`, Security group `tracker-ec2-sg`.
4. Nâng cao: IAM instance profile $\rightarrow$ Chọn `tracker-ec2-role`.
5. Tạo 01 **Elastic IP** và Associate trực tiếp vào máy chủ EC2 (`3.106.194.112`).
6. SSH vào EC2 và cài đặt Docker & Docker Compose:
   ```bash
   sudo yum install docker -y
   sudo systemctl enable docker && sudo systemctl start docker
   sudo usermod -aG docker ec2-user
   ```

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình EC2 Instance `tracker-ec2-server` cùng Elastic IP đã gắn đính kèm vào bên dưới.
> 
> ![Khởi tạo EC2 Instance](/images/5-Workshop/5.3-S3-vpc/ec2-instance-setup.png?classes=shadow)

---

### Bước 5.3.8: Cấu hình GitHub Secrets & CI/CD Pipeline

1. Trong GitHub Repository $\rightarrow$ Vào **Settings** $\rightarrow$ **Secrets and variables** $\rightarrow$ **Actions**.
2. Thêm các Secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `EC2_HOST`, `EC2_SSH_KEY`.
3. Push file `.github/workflows/deploy.yml` lên nhánh `main` để kích hoạt luồng tự động build Docker, push ECR và deploy EC2.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình GitHub Actions tích xanh chạy thành công luồng `Deploy to EC2` đính kèm vào bên dưới.
> 
> ![Cấu hình GitHub Actions](/images/5-Workshop/5.3-S3-vpc/github-actions-setup.png?classes=shadow)

---

### Bước 5.3.9: Cấu hình Route 53 DNS & Chứng chỉ ACM SSL

1. Mở **Route 53 Console** $\rightarrow$ Tạo Hosted Zone cho `trackermaint.dpdns.org`.
2. Tạo **A Record** trỏ tên miền `trackermaint.dpdns.org` về Elastic IP của EC2 (`3.106.194.112`).
3. Khởi tạo chứng chỉ **ACM Certificate** tại vùng `us-east-1` (N. Virginia) cho `trackermaint.dpdns.org` để chuẩn bị cho CloudFront.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình Route 53 A Record trỏ IP đính kèm vào bên dưới.
> 
> ![Cấu hình Route 53](/images/5-Workshop/5.3-S3-vpc/route53-acm-setup.png?classes=shadow)