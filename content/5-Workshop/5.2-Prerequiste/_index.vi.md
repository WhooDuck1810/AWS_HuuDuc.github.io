---
title: "Chuẩn bị"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 2. Chuẩn bị

## 2.1 Yêu cầu kiến thức

Trước khi bắt đầu bài lab này, người tham gia cần trang bị các kiến thức nền tảng sau:

- **Dòng lệnh Linux cơ bản:** Khả năng SSH vào máy chủ, thao tác di chuyển thư mục và chạy các lệnh shell.
- **Kiến thức Docker cơ bản:** Hiểu khái niệm Docker Image, Container, cách vận hành file `docker-compose.yml` và các lệnh cơ bản (`docker pull`, `docker ps`, `docker logs`).
- **Khái niệm REST API:** Hiểu các phương thức HTTP (GET, POST, PUT, DELETE), mã trạng thái HTTP và định dạng dữ liệu JSON.
- **Thao tác trên AWS Console:** Khả năng đăng nhập AWS Console, chuyển đổi vùng (Region) và di chuyển giữa các dịch vụ.
- **Kỹ năng Git cơ bản:** Khả năng `git commit`, `git push` và hiểu mô hình làm việc với repository trên GitHub.
- **Kiến thức Spring Boot (Khuyến khích):** Nắm cấu trúc Spring MVC, các annotation `@RestController`, `@Service`, `@Repository`.
- **Kiến thức ReactJS (Khuyến khích):** Nắm cấu trúc component, React Hooks (`useState`, `useEffect`, `useContext`) và định tuyến Route.

## 2.2 Yêu cầu Hạ tầng & Tài nguyên

Các tài nguyên AWS sau đây cần được khởi tạo **trước** khi tiến hành các bước triển khai chi tiết:

### 2.2.1 Tài khoản AWS
- 01 **Tài khoản AWS** đang hoạt động có quyền Administrator.
- Tài khoản cần có quyền truy cập các dịch vụ: EC2, RDS, S3, ECR, CloudWatch, IAM, Route 53, CloudFront, ACM.

### 2.2.2 Máy chủ Amazon EC2
- **Loại Instance:** `t2.micro` (1 vCPU, 1 GB RAM) — Miễn phí thuộc gói AWS Free Tier.
- **Vùng (Region):** `ap-southeast-2` (Sydney, Australia).
- **Hệ điều hành (AMI):** Amazon Linux 2023.
- **Lưu trữ:** Ổ cứng 20 GB gp3 EBS.
- **Key Pair:** File khóa `.pem` lưu trên máy cá nhân để kết nối SSH (ví dụ: `tracker-key.pem`).
- **Security Group (Quy tắc Inbound):**

| Cổng (Port) | Giao thức | Nguồn (Source) | Mục đích |
|---|---|---|---|
| 22 | TCP | IP của bạn (hoặc GitHub Actions IP) | Truy cập SSH |
| 80 | TCP | 0.0.0.0/0 | Luồng web HTTP |
| 443 | TCP | 0.0.0.0/0 | Luồng web HTTPS |
| 3000 | TCP | 0.0.0.0/0 | Ứng dụng Frontend React |
| 8081 | TCP | 0.0.0.0/0 | Backend Spring Boot API |

- **Phần mềm cài sẵn trên EC2:**
  - Docker Engine (`sudo yum install docker -y && sudo systemctl enable docker && sudo systemctl start docker`)
  - Docker Compose plugin (`sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose`)
  - AWS CLI v2 (đã tích hợp sẵn trên Amazon Linux 2023)

### 2.2.3 Cơ sở dữ liệu Amazon RDS PostgreSQL
- **Engine:** PostgreSQL 15.
- **Cấu hình Instance:** `db.t4g.micro` (2 vCPU, 1 GB RAM).
- **Vùng (Region):** `ap-southeast-2` (Sydney).
- **Multi-AZ:** Tắt (Single-AZ để tối ưu chi phí).
- **Public Accessibility:** Tắt (Chỉ nằm trong Private Subnet, kết nối nội bộ từ EC2 trong cùng VPC).
- **Tên Database:** `postgres`
- **Master Username:** `postgres`
- **Endpoint:** `tracker-maintenance-db.cvow26so4q44.ap-southeast-2.rds.amazonaws.com:5432`
- **Security Group:** Cho phép cổng 5432 (PostgreSQL) từ Security Group của máy chủ EC2.

### 2.2.4 Amazon S3 Bucket
- **Tên Bucket:** `tracker-maintenance-images-123`
- **Vùng (Region):** `ap-southeast-2` (Sydney).
- **Cấu hình Block Public Access:** **Tắt (Disabled)** để cho phép đọc URL ảnh công khai.
- **Bucket Policy:** Cho phép quyền `s3:GetObject` công khai cho mọi object.
- **Cấu hình CORS:** Cho phép các phương thức `PUT` và `GET` từ tên miền ứng dụng.

### 2.2.5 Kho lưu trữ Amazon ECR
Khởi tạo 02 ECR Repository riêng tư tại vùng `ap-southeast-2`:
- `tracker-be` — Lưu trữ Docker Image của Backend Spring Boot.
- `tracker-fe` — Lưu trữ Docker Image của Frontend React.

### 2.2.6 Thiết lập Phân quyền AWS IAM
- **IAM User cho GitHub Actions CI/CD:**
  - Quyền truy cập lập trình (Access Key ID + Secret Access Key).
  - Quyền gắn kèm: `AmazonEC2ContainerRegistryFullAccess` (để push Docker image lên ECR).
  - Lưu vào GitHub Secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`.
- **IAM Role cho máy chủ EC2:**
  - Gắn trực tiếp vào EC2 Instance.
  - Quyền gắn kèm: `AmazonEC2ContainerRegistryReadOnly` (để pull Docker image từ ECR), `CloudWatchAgentServerPolicy` (để đẩy log về CloudWatch).

### 2.2.7 Tên miền và DNS
- **Tên miền:** `trackermaint.dpdns.org` — Đăng ký và quản lý qua nhà cung cấp DNS miễn phí DuckDNS.
- **Route 53 Hosted Zone:** Cấu hình A Record trỏ tên miền `trackermaint.dpdns.org` về Elastic IP của EC2.
- **Chứng chỉ SSL ACM:** Khởi tạo cho `trackermaint.dpdns.org` tại vùng `us-east-1 (N. Virginia)` để tích hợp CloudFront.

### 2.2.8 Môi trường máy cá nhân (Local Dev)
- **Hệ điều hành:** Windows 10/11.
- **JDK:** Java 21 (Amazon Corretto hoặc Temurin).
- **Node.js:** v18 hoặc v20 LTS.
- **Maven Wrapper:** `mvnw.cmd` có sẵn trong dự án Spring Boot.
- **Docker Desktop:** Cài đặt để build và test container cục bộ.
- **Git:** Đã cấu hình SSH key kết nối với GitHub.
- **SSH Key:** File `tracker-key.pem` để truy cập máy chủ EC2.
- **IDE:** IntelliJ IDEA (cho Backend) và Visual Studio Code (cho Frontend).

### 2.2.9 GitHub Repository & Secrets

GitHub Repository: `https://github.com/WhooDuck1810/TrackerMaintenance`

Bảng danh sách GitHub Secrets bắt buộc:
| Tên Secret | Mô tả |
|---|---|
| `AWS_ACCESS_KEY_ID` | Access Key của IAM User để push ECR |
| `AWS_SECRET_ACCESS_KEY` | Secret Key của IAM User để push ECR |
| `EC2_HOST` | IP công khai của EC2 (`3.106.194.112`) |
| `EC2_SSH_KEY` | Nội dung file private key `.pem` để SSH vào EC2 |
| `APP_R2_ACCESS_KEY_ID` | Access Key AWS để upload ảnh lên S3 |
| `APP_R2_SECRET_ACCESS_KEY` | Secret Key AWS để upload ảnh lên S3 |
