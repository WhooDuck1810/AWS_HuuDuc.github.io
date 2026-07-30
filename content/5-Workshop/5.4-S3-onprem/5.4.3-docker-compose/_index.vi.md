---
title: "Đóng gói Docker Multi-Stage & Docker Compose trên EC2"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

# 5.4.3 Đóng gói Docker Multi-Stage & Docker Compose trên EC2

Trong bước này, đóng gói cả 2 ứng dụng Backend và Frontend thành các Docker Container tối ưu bộ nhớ RAM và điều phối bằng Docker Compose.

### Backend Dockerfile (Multi-Stage)
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

### Cấu hình Docker Compose từ xa (`docker-compose-remote.yml`)
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
