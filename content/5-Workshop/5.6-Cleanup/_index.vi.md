---
title: "Khó khăn & Hướng phát triển"
date: 2026-07-30
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. Khó khăn & Hướng phát triển

## 1. Khó khăn gặp phải & Giải pháp

### 1.1 Lỗi đường dẫn Windows vs. Linux khi Build React Router
**Vấn đề:** Khi chạy `npm run build` trên hệ điều hành Windows, file bundle `build/server/index.js` bị nhúng các dấu gạch ngược đường dẫn kiểu Windows (`build\client`). Khi đưa Docker image lên môi trường Linux container trên EC2, thư viện `@react-router/serve` không thể tìm thấy tài nguyên, gây ra lỗi HTTP 404 cho toàn bộ file JS/CSS.

**Giải pháp:** Chuyển toàn bộ quy trình build Docker Image sang môi trường Linux Runner của GitHub Actions. Việc build trên Ubuntu giúp các ký tự phân cách đường dẫn luôn là dấu gạch chéo `/` chuẩn Linux. Nguyên tắc: **Không bao giờ build Docker Image của Frontend trực tiếp trên Windows**.

### 1.2 Giới hạn bộ nhớ RAM trên EC2 Free Tier (1 GB RAM)
**Vấn đề:** Chạy đồng thời cả Backend Spring Boot (JVM) và Frontend React (Node.js) trên instance `t2.micro` (1 GB RAM) dẫn đến tình trạng tràn bộ nhớ RAM (OOM) làm container bị sập.

**Giải pháp:** Cấu hình giới hạn bộ nhớ heap cho JVM trong `docker-compose.yml`:
```yaml
JAVA_TOOL_OPTIONS: "-Xms256m -Xmx512m"
```
Giới hạn bộ nhớ heap tối đa ở mức 512 MB giúp hệ thống luôn còn đủ RAM trống cho Node.js và các tiến trình của hệ điều hành Linux.

### 1.3 Yêu cầu xác minh tài khoản khi cấu hình CloudFront
**Vấn đề:** Khi tạo CloudFront Distribution, AWS Console hiển thị thông báo lỗi yêu cầu xác minh tài khoản (*"Your account must be verified before you can add new CloudFront resources"*).

**Tác động:** Việc kích hoạt CloudFront CDN tạm thời chờ phản hồi xác minh từ AWS Support. Tuy nhiên, các bước chuẩn bị (ACM SSL tại `us-east-1`, Route 53 DNS) đã hoàn tất 100%. Ứng dụng vẫn hoạt động bình thường qua kết nối trực tiếp Route 53.

### 1.4 Kết nối mạng giữa EC2 và RDS Private Subnet
**Vấn đề:** Ban đầu máy chủ EC2 không thể kết nối tới cơ sở dữ liệu RDS trong Private Subnet (bị lỗi Connection Timeout).

**Giải pháp:** Cập nhật quy tắc Inbound của RDS Security Group, cho phép kết nối cổng 5432 trực tiếp từ ID Security Group của EC2.

### 1.5 Lỗi CORS WebSocket phía sau Proxy
**Vấn đề:** Kết nối SockJS WebSocket từ trình duyệt bị từ chối do lỗi CORS khi gọi qua tên miền `trackermaint.dpdns.org`.

**Giải pháp:** Cập nhật `WebSocketConfig` của Spring Boot với `setAllowedOriginPatterns("*")` và bổ sung tên miền `https://trackermaint.dpdns.org` vào danh sách Allowed Origins của `SecurityConfig`.

---

## 2. Hướng phát triển trong tương lai

### 2.1 Triển khai Đa vùng Active-Active (Multi-Region)
- Khởi tạo thêm máy chủ EC2 tại vùng `ap-southeast-1` (Singapore) và `us-east-2` (Ohio).
- Sử dụng chiến lược Matrix Deployment của GitHub Actions để deploy tự động đồng thời lên cả 3 máy chủ EC2.
- Cấu hình Latency-Based Routing trên AWS Route 53 để tự động điều hướng người dùng đến máy chủ có độ trễ thấp nhất.

### 2.2 Tích hợp Amazon CloudFront CDN
- Hoàn tất kích hoạt CloudFront Distribution cho S3 và Frontend sau khi AWS xác minh tài khoản.
- Áp dụng Origin Access Control (OAC) để bảo mật luồng truy cập S3.

### 2.3 Nâng cấp cơ sở dữ liệu Amazon Aurora Global Database
- Chuyển đổi từ RDS PostgreSQL sang Amazon Aurora PostgreSQL bằng dịch vụ AWS DMS.
- Cấu hình Aurora Global Database với cluster chính tại Sydney và các cluster Read-Replica tại Singapore và Ohio.

### 2.4 Tự động hóa hạ tầng bằng Infrastructure as Code (AWS CDK)
- Định nghĩa 100% tài nguyên AWS bằng mã nguồn TypeScript sử dụng **AWS CDK v2**.
- Tự động hóa việc khởi tạo toàn bộ môi trường Cloud chỉ với 1 lệnh (`cdk deploy`).

### 2.5 Tự động mở rộng (Auto Scaling Group & Load Balancer)
- Thay thế EC2 đơn lẻ bằng nhóm Auto Scaling Group (ASG) đặt phía sau Application Load Balancer (ALB).
- Tự động tăng/giảm số lượng máy chủ EC2 dựa trên tải CPU thực tế.

### 2.6 Giám sát nâng cao với CloudWatch Dashboards & Alarms
- Xây dựng CloudWatch Dashboard theo dõi trực quan hiệu năng hệ thống.
- Cấu hình cảnh báo tự động qua Email/SMS khi phát hiện tấn công dò mật khẩu (tăng đột biến lỗi HTTP 429).

### 2.7 Lập lịch bảo trì tự động với AWS EventBridge
- Tự động gửi thông báo nhắc nhở lịch bảo trì định kỳ sắp tới qua AWS EventBridge Scheduler kết hợp Amazon SES và WebSocket.

---

## Tổng kết bài lab

Bài lab đã hoàn thành xuất sắc việc triển khai một ứng dụng Web Full-Stack hoàn chỉnh trên nền tảng Cloud AWS. Hệ thống **Tracker Maintenance System** tích hợp thành công hơn 10 dịch vụ AWS:

- ✅ **Xác thực an toàn** với JWT + Chống tấn công Brute-Force (Khóa HTTP 429)
- ✅ **Máy chủ Cloud** trên Amazon EC2 đóng gói bằng Docker
- ✅ **Cơ sở dữ liệu quản lý** trên Amazon RDS PostgreSQL (Private Subnet)
- ✅ **Lưu trữ Cloud** trên Amazon S3 cho hình ảnh thiết bị
- ✅ **Thông báo thời gian thực** qua WebSocket (SockJS/STOMP)
- ✅ **Quản lý tài sản qua mã QR** với trang tra cứu công khai
- ✅ **Tự động hóa CI/CD** qua GitHub Actions → ECR → EC2
- ✅ **Sao chép ECR đa vùng** qua ECR Replication Rules
- ✅ **Quản lý Log tập trung** trên CloudWatch Logs
- ✅ **Tên miền & SSL toàn cầu** qua Route 53 + Chứng chỉ ACM (`us-east-1`)
- 🔄 **CloudFront CDN** — Đã cấu hình, chờ AWS xác minh tài khoản

Hệ thống hiện đang hoạt động trực tuyến tại địa chỉ **https://trackermaint.dpdns.org**.