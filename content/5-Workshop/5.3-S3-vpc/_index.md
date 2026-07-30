---
title: "Implementation Steps"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 3. Implementation Steps

## 3.1 Step 1: Full-Stack Web Application Development

### 3.1.1 Backend — Spring Boot REST API

The backend is built using **Spring Boot 3.5** with **Java 21** and follows a clean layered architecture:

**Directory Structure:**
```
tracker_maintenance_service/
├── src/main/java/com/procare_system/tracker_maintenance_service/
│   ├── configuration/         # Spring Security, CORS, Beans
│   │   ├── ApplicationInitConfig.java   # Seeds default admin user
│   │   └── SecurityConfig.java          # JWT filter chain
│   ├── controller/            # REST API endpoints
│   │   ├── AuthenticationController.java
│   │   ├── EquipmentController.java
│   │   ├── TicketController.java
│   │   ├── UserController.java
│   │   ├── ScheduleController.java
│   │   └── NotificationController.java
│   ├── service/               # Business logic layer
│   │   ├── AuthenticationService.java
│   │   ├── LoginAttemptService.java     # Brute-force protection
│   │   ├── EquipmentService.java
│   │   ├── TicketService.java
│   │   └── NotificationService.java
│   ├── entity/                # JPA entities (DB tables)
│   ├── repository/            # Spring Data JPA repositories
│   ├── exception/             # Custom exceptions & error codes
│   │   └── ErrorCode.java     # TOO_MANY_LOGIN_ATTEMPTS (HTTP 429)
│   └── dto/                   # Request/Response DTOs
└── src/main/resources/
    └── application.yml        # DB, JWT, S3 config via env vars
```

**Key API Endpoints:**

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/auth/token` | Public | Login and receive JWT token |
| `GET` | `/users` | ADMIN | List all users |
| `POST` | `/users` | ADMIN | Create a new user |
| `GET` | `/equipment` | AUTH | List all equipment |
| `POST` | `/equipment` | MANAGER | Create equipment with QR code |
| `GET` | `/equipment/{id}` | AUTH | Get equipment details |
| `GET` | `/public/equipment/{id}` | Public | QR code public equipment lookup |
| `GET` | `/tickets` | AUTH | List maintenance tickets |
| `POST` | `/tickets` | REPORTER/MANAGER | Create a new ticket |
| `PATCH` | `/tickets/{id}/status` | TECHNICIAN/MANAGER | Update ticket status |
| `GET` | `/notifications` | AUTH | Get user notifications |
| `PATCH` | `/notifications/{id}/read` | AUTH | Mark notification as read |
| `GET` | `/schedules` | MANAGER | List maintenance schedules |

### 3.1.2 Frontend — React Router v7 with TypeScript

The frontend is built using **React Router v7** in **Server-Side Rendering (SSR)** mode with TypeScript and TailwindCSS.

**Route Structure:**
```
/ ──► (redirect to /login)
/login ──► LoginPage
/admin
  /dashboard ──► Admin Dashboard
  /users ──► User Management
/manager
  /dashboard ──► Manager Dashboard
  /equipment ──► Equipment List
  /equipment/:id ──► Equipment Detail + QR Code
  /tickets ──► Ticket Management (all tickets)
  /schedules ──► Maintenance Schedule
  /reports ──► Reports & Analytics
/technician
  /my-tickets ──► Assigned Tickets for Technician
/reporter
  /my-tickets ──► Submitted Tickets for Reporter
  /report-issue ──► New Ticket Submission Form
/public
  /equipment/:id ──► Public Equipment Scan Page (no login required)
```

**Key Frontend Components:**
- **`AppLayout.tsx`:** Main layout wrapper with sidebar navigation, header with notification bell, and role-based menu items.
- **`NotificationDropdown.tsx`:** Dropdown component showing unread notifications, connected to real-time WebSocket updates.
- **`NotificationContext.tsx`:** React Context providing shared notification state, unread count, and WebSocket connection lifecycle across the entire app.
- **`QrCodeDisplay.tsx`:** Renders a QR code for each equipment item using the `qrcode.react` library.
- **`favicon.svg`:** A custom SVG favicon with a gradient shield and wrench/checkmark icon representing the maintenance theme.

**Browser Tab Title Decorator:**
The `NotificationContext` dynamically updates `document.title` based on the active route and unread notification count:
- Default: `Tracker Maintenance`
- With unread notifications: `(3) Ticket Management | Tracker Maintenance`

### 3.1.3 Role-Based Access Control (RBAC)

The system supports 4 user roles with distinct permissions:

| Role | Permissions |
|---|---|
| **ADMIN** | Full system access: manage users, view all data, system configuration |
| **MANAGER** | Manage equipment, create/assign/close tickets, manage schedules, view reports |
| **TECHNICIAN** | View assigned tickets, update ticket status, upload maintenance evidence photos |
| **REPORTER** | Submit new fault reports (tickets), view their own submitted tickets |

## 3.2 Step 2: Brute-Force Anti-Login Protection

A custom `LoginAttemptService` was implemented using Java's thread-safe `ConcurrentHashMap` to track login attempts without any external cache dependency:

**Logic (`LoginAttemptService.java`):**
```java
private final ConcurrentHashMap<String, AttemptRecord> usernameAttempts = new ConcurrentHashMap<>();
private final ConcurrentHashMap<String, AttemptRecord> ipAttempts = new ConcurrentHashMap<>();

private static final int MAX_USERNAME_ATTEMPTS = 5;   // Lock username after 5 failures
private static final int MAX_IP_ATTEMPTS = 20;         // Lock IP after 20 failures
private static final long LOCK_DURATION_MS = 15 * 60 * 1000; // 15-minute lockout
```

**Error Code (`ErrorCode.java`):**
```java
TOO_MANY_LOGIN_ATTEMPTS(
    "Too many failed login attempts. Your account has been temporarily locked. Please try again in 15 minutes.",
    HttpStatus.TOO_MANY_REQUESTS  // HTTP 429
)
```

## 3.3 Step 3: Docker Containerization with Multi-Stage Builds

### 3.3.1 Backend Dockerfile (Multi-Stage)
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

### 3.3.2 Frontend Dockerfile (Multi-Stage)
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

### 3.3.3 Docker Compose (`docker-compose-remote.yml`)
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

## 3.4 Step 4: CI/CD Pipeline with GitHub Actions

Workflow file `.github/workflows/deploy.yml`:

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

## 3.5 Step 5: Amazon S3 — Cloud Image Storage

All equipment photos and maintenance evidence images are uploaded directly to **Amazon S3** from the Spring Boot backend using the **AWS SDK for Java v2**.

1. Technician/Manager sends image via `POST /equipment/{id}/images`.
2. Spring Boot `EquipmentService` uploads file using `S3Client.putObject()`.
3. S3 returns public URL (`https://tracker-maintenance-images-123.s3.ap-southeast-2.amazonaws.com/equipment/eq-001/photo.jpg`).
4. URL is saved into PostgreSQL database.

## 3.6 Step 6: Real-Time Notifications via WebSocket

Backend uses **Spring WebSocket** with **STOMP messaging protocol** over **SockJS**.
Frontend `NotificationContext.tsx` listens on `/user/queue/notifications` and updates unread badge count & document title dynamically.

## 3.7 Step 7: QR Code Equipment Tracking

Each equipment item has an auto-generated QR code pointing to:
`https://trackermaint.dpdns.org/public/equipment/{equipmentId}`

Anyone scanning the physical QR code can instantly view equipment details, current operational status, and maintenance history without needing to log in.

## 3.8 Step 8: CloudWatch Centralized Logging

Configured `awslogs` driver streams stdout/stderr to CloudWatch Log Groups:
- `/tracker-maintenance/backend`
- `/tracker-maintenance/frontend`

## 3.9 Step 9: ECR Cross-Region Replication

Configured ECR replication rules in Sydney registry to automatically sync images to `ap-southeast-1` (Singapore) and `us-east-2` (Ohio).

## 3.10 Step 10: DNS and SSL with Route 53 and ACM

- **Domain:** `trackermaint.dpdns.org`
- **Route 53 A Record:** Points `trackermaint.dpdns.org` to EC2 Elastic IP (`3.106.194.112`).
- **ACM SSL Certificate:** Provisioned in `us-east-1` for CloudFront integration.