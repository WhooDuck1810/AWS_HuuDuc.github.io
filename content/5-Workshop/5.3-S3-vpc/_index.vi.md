---
title: "Các bước thực hiện"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 3. Các bước thực hiện

## 3.1 Bước 1: Phát triển ứng dụng Web Full-Stack

### 3.1.1 Backend — Spring Boot REST API

Backend được xây dựng trên **Spring Boot 3.5** sử dụng **Java 21** với kiến trúc phân tầng chuẩn:

**Cấu trúc thư mục:**
```
tracker_maintenance_service/
├── src/main/java/com/procare_system/tracker_maintenance_service/
│   ├── configuration/         # Cấu hình Security, CORS, Beans
│   │   ├── ApplicationInitConfig.java   # Khởi tạo tài khoản Admin mặc định
│   │   └── SecurityConfig.java          # Cấu hình chuỗi lọc JWT Filter Chain
│   ├── controller/            # Các endpoint REST API
│   │   ├── AuthenticationController.java
│   │   ├── EquipmentController.java
│   │   ├── TicketController.java
│   │   ├── UserController.java
│   │   ├── ScheduleController.java
│   │   └── NotificationController.java
│   ├── service/               # Tầng xử lý logic nghiệp vụ
│   │   ├── AuthenticationService.java
│   │   ├── LoginAttemptService.java     # Bảo vệ chống tấn công Brute-force
│   │   ├── EquipmentService.java
│   │   ├── TicketService.java
│   │   └── NotificationService.java
│   ├── entity/                # Các JPA Entity (Bảng trong DB)
│   ├── repository/            # Tầng truy vấn dữ liệu Spring Data JPA
│   ├── exception/             # Xử lý ngoại lệ & Mã lỗi tùy chỉnh
│   │   └── ErrorCode.java     # TOO_MANY_LOGIN_ATTEMPTS (HTTP 429)
│   └── dto/                   # Đối tượng truyền dữ liệu Request/Response DTO
└── src/main/resources/
    └── application.yml        # Cấu hình DB, JWT, S3 qua biến môi trường
```

**Các API Endpoint chính:**

| Phương thức | Endpoint | Quyền truy cập | Mô tả |
|---|---|---|---|
| `POST` | `/auth/token` | Công khai | Đăng nhập và nhận JWT token |
| `GET` | `/users` | ADMIN | Lấy danh sách người dùng |
| `POST` | `/users` | ADMIN | Tạo người dùng mới |
| `GET` | `/equipment` | Đã đăng nhập | Lấy danh sách thiết bị |
| `POST` | `/equipment` | MANAGER | Tạo mới thiết bị kèm mã QR |
| `GET` | `/equipment/{id}` | Đã đăng nhập | Xem chi tiết thiết bị |
| `GET` | `/public/equipment/{id}` | Công khai | Tra cứu thiết bị qua mã QR không cần đăng nhập |
| `GET` | `/tickets` | Đã đăng nhập | Xem danh sách phiếu bảo trì |
| `POST` | `/tickets` | REPORTER/MANAGER | Tạo phiếu yêu cầu bảo trì mới |
| `PATCH` | `/tickets/{id}/status` | TECHNICIAN/MANAGER | Cập nhật trạng thái phiếu |
| `GET` | `/notifications` | Đã đăng nhập | Lấy danh sách thông báo |
| `PATCH` | `/notifications/{id}/read` | Đã đăng nhập | Đánh dấu thông báo đã đọc |
| `GET` | `/schedules` | MANAGER | Xem lịch bảo trì định kỳ |

### 3.1.2 Frontend — React Router v7 với TypeScript

Frontend được phát triển bằng **React Router v7** ở chế độ **Server-Side Rendering (SSR)** tích hợp TypeScript và TailwindCSS.

**Cấu trúc đường dẫn (Routes):**
```
/ ──► (Tự động chuyển hướng sang /login)
/login ──► Trang đăng nhập
/admin
  /dashboard ──► Dashboard Quản trị viên
  /users ──► Quản lý người dùng
/manager
  /dashboard ──► Dashboard Quản lý
  /equipment ──► Danh sách thiết bị
  /equipment/:id ──► Chi tiết thiết bị + Mã QR
  /tickets ──► Quản lý tất cả phiếu bảo trì
  /schedules ──► Lịch bảo trì định kỳ
  /reports ──► Báo cáo & Thống kê
/technician
  /my-tickets ──► Phiếu bảo trì được phân công cho Kỹ thuật viên
/reporter
  /my-tickets ──► Phiếu sự cố đã gửi của Người báo cáo
  /report-issue ──► Biểu mẫu gửi báo cáo sự cố mới
/public
  /equipment/:id ──► Trang công khai quét mã QR thiết bị (không cần login)
```

### 3.1.3 Phân quyền người dùng (RBAC)

Hệ thống hỗ trợ 4 vai trò với phân quyền rõ ràng:

| Vai trò | Quyền hạn |
|---|---|
| **ADMIN** | Toàn quyền hệ thống: Quản lý người dùng, xem dữ liệu, cấu hình hệ thống |
| **MANAGER** | Quản lý thiết bị, tạo/phân công/đóng ticket, quản lý lịch bảo trì, xem báo cáo |
| **TECHNICIAN** | Xem ticket được phân công, cập nhật trạng thái ticket, tải ảnh nghiệm thu |
| **REPORTER** | Gửi báo cáo sự cố thiết bị mới, theo dõi các ticket do chính mình gửi |

## 3.2 Bước 2: Bảo mật chống tấn công Brute-Force Đăng nhập

Triển khai dịch vụ `LoginAttemptService` sử dụng `ConcurrentHashMap` trên RAM để đếm số lần đăng nhập thất bại:

**Logic xử lý (`LoginAttemptService.java`):**
```java
private final ConcurrentHashMap<String, AttemptRecord> usernameAttempts = new ConcurrentHashMap<>();
private final ConcurrentHashMap<String, AttemptRecord> ipAttempts = new ConcurrentHashMap<>();

private static final int MAX_USERNAME_ATTEMPTS = 5;   // Khóa username sau 5 lần sai
private static final int MAX_IP_ATTEMPTS = 20;         // Khóa IP sau 20 lần sai
private static final long LOCK_DURATION_MS = 15 * 60 * 1000; // Khóa trong 15 phút
```

**Mã lỗi HTTP 429 (`ErrorCode.java`):**
```java
TOO_MANY_LOGIN_ATTEMPTS(
    "Quá nhiều lần đăng nhập thất bại. Tài khoản của bạn đã bị khóa tạm thời. Vui lòng thử lại sau 15 phút.",
    HttpStatus.TOO_MANY_REQUESTS  // HTTP 429
)
```

## 3.3 Bước 3: Đóng gói Container với Docker Multi-Stage Builds

### 3.3.1 Dockerfile cho Backend
```dockerfile
FROM maven:3.9-amazoncorretto-21 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

FROM amazoncorretto:21-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### 3.3.2 Dockerfile cho Frontend
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json .
RUN npm ci
COPY . .
ARG VITE_API_BASE_URL
ENV VITE_API_BASE_URL=$VITE_API_BASE_URL
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/build ./build
COPY --from=builder /app/package*.json .
RUN npm ci --omit=dev
EXPOSE 3000
CMD ["node", "node_modules/@react-router/serve/dist/index.js", "build/server/index.js"]
```

### 3.3.3 Docker Compose trên máy chủ EC2 (`docker-compose-remote.yml`)
```yaml
services:
  tracker-be:
    image: 534120921488.dkr.ecr.ap-southeast-2.amazonaws.com/tracker-be:latest
    ports:
      - "8081:8081"
    environment:
      JAVA_TOOL_OPTIONS: "-Xms256m -Xmx512m"
      DB_URL: jdbc:postgresql://tracker-maintenance-db.cvow26so4q44.ap-southeast-2.rds.amazonaws.com:5432/postgres
      DB_USERNAME: postgres
      DB_PASSWORD: admin1810
      JWT_SIGNER_KEY: myTrackerMaintenanceSecretKeyForHS512AlgorithmMustBe64CharsLong!
      APP_BASE_URL: https://trackermaint.dpdns.org
      CLOUDFLARE_R2_ENDPOINT: https://s3.ap-southeast-2.amazonaws.com
      CLOUDFLARE_R2_PUBLICURL: https://tracker-maintenance-images-123.s3.ap-southeast-2.amazonaws.com
      CLOUDFLARE_R2_BUCKETNAME: tracker-maintenance-images-123
      APP_R2_ACCESS_KEY_ID: ${APP_R2_ACCESS_KEY_ID}
      APP_R2_SECRET_ACCESS_KEY: ${APP_R2_SECRET_ACCESS_KEY}
      APP_R2_REGION: ap-southeast-2
    logging:
      driver: awslogs
      options:
        awslogs-region: ap-southeast-2
        awslogs-group: /tracker-maintenance/backend
        awslogs-create-group: "true"
        awslogs-stream: be-logs
    restart: on-failure

  tracker-fe:
    image: 534120921488.dkr.ecr.ap-southeast-2.amazonaws.com/tracker-fe:latest
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: production
    logging:
      driver: awslogs
      options:
        awslogs-region: ap-southeast-2
        awslogs-group: /tracker-maintenance/frontend
        awslogs-create-group: "true"
        awslogs-stream: fe-logs
    depends_on:
      - tracker-be
    restart: on-failure
```

## 3.4 Bước 4: Tự động hóa luồng CI/CD với GitHub Actions

File cấu hình `.github/workflows/deploy.yml`:

```yaml
name: Deploy to EC2 (Tracker Maintenance)

on:
  push:
    branches:
      - main

env:
  AWS_REGION: ap-southeast-2
  ECR_REGISTRY: 534120921488.dkr.ecr.ap-southeast-2.amazonaws.com
  BE_REPO: tracker-be
  FE_REPO: tracker-fe

jobs:
  build-and-push:
    name: Build and Push to AWS ECR
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and Push Backend
        run: |
          cd tracker_maintenance_service
          docker build -t $ECR_REGISTRY/$BE_REPO:latest .
          docker push $ECR_REGISTRY/$BE_REPO:latest

      - name: Build and Push Frontend
        run: |
          cd tracker_maintenance_ui
          docker build \
            --build-arg VITE_API_BASE_URL=https://trackermaint.dpdns.org \
            -t $ECR_REGISTRY/$FE_REPO:latest .
          docker push $ECR_REGISTRY/$FE_REPO:latest

  deploy:
    name: Deploy to EC2
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - name: SSH into EC2 and Restart Containers
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ec2-user
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd /home/ec2-user
            aws ecr get-login-password --region ap-southeast-2 \
              | docker login --username AWS \
              --password-stdin 534120921488.dkr.ecr.ap-southeast-2.amazonaws.com
            docker-compose pull
            docker-compose up -d
            docker image prune -f
```

## 3.5 Bước 5: Amazon S3 — Lưu trữ hình ảnh trên Cloud

Toàn bộ ảnh thiết bị và ảnh nghiệm thu được tải lên trực tiếp **Amazon S3** từ Spring Boot bằng **AWS SDK cho Java v2**:

1. Người dùng gửi file ảnh qua API `POST /equipment/{id}/images`.
2. Spring Boot `EquipmentService` gọi `S3Client.putObject()` để lưu vào S3.
3. S3 trả về URL công khai (`https://tracker-maintenance-images-123.s3.ap-southeast-2.amazonaws.com/equipment/eq-001/photo.jpg`).
4. Đường dẫn URL này được lưu lại vào cơ sở dữ liệu PostgreSQL.

## 3.6 Bước 6: Thông báo thời gian thực qua WebSocket

Backend sử dụng **Spring WebSocket** với **giao thức STOMP** truyền qua **SockJS**.
Frontend `NotificationContext.tsx` lắng nghe trên kênh `/user/queue/notifications` để tự động cập nhật badge số lượng thông báo chưa đọc và tiêu đề thẻ trình duyệt theo thời gian thực.

## 3.7 Bước 7: Quản lý thiết bị qua Mã QR Code

Mỗi thiết bị khi khởi tạo sẽ tự động có một mã QR trỏ đến liên kết công khai:
`https://trackermaint.dpdns.org/public/equipment/{equipmentId}`

Kỹ thuật viên tại hiện trường chỉ cần dùng camera smartphone quét mã QR dán trên máy là có thể xem ngay chi tiết thông tin, trạng thái hoạt động và lịch sử bảo trì mà không cần đăng nhập.

## 3.8 Bước 8: Quản lý Log tập trung với CloudWatch

Cấu hình driver `awslogs` trong Docker Compose để tự động đẩy stream log về CloudWatch Log Groups:
- `/tracker-maintenance/backend`
- `/tracker-maintenance/frontend`

## 3.9 Bước 9: Sao chép ECR đa vùng (Cross-Region Replication)

Cấu hình quy tắc sao chép ECR từ vùng chính Sydney để tự động đồng bộ Docker image sang vùng `ap-southeast-1` (Singapore) và `us-east-2` (Ohio).

## 3.10 Bước 10: Tên miền và SSL với Route 53 và ACM

- **Tên miền:** `trackermaint.dpdns.org`
- **Route 53 A Record:** Trỏ `trackermaint.dpdns.org` về Elastic IP của EC2 (`3.106.194.112`).
- **Chứng chỉ ACM SSL:** Khởi tạo tại vùng `us-east-1` sẵn sàng tích hợp CloudFront.