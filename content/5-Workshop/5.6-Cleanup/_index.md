---
title: "Difficulties & Development Direction"
date: 2026-07-30
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. Difficulties & Development Direction

## 1. Difficulties Encountered & Solutions

### 1.1 Windows vs. Linux Path Issue in React Router Build
**Problem:** Running `npm run build` on a Windows developer machine caused `build/server/index.js` to contain hardcoded Windows backslash paths (`build\client`). When the Docker image was run on a Linux container, `@react-router/serve` could not resolve asset paths, resulting in HTTP 404 errors for all JavaScript and CSS files.

**Solution:** Moved the build process entirely to the GitHub Actions Linux runner. By pushing source code to GitHub and letting the CI/CD pipeline build the Docker image on Ubuntu (Linux), all path separators are forward slashes and the container functions properly. Rule: **never build the frontend Docker image locally on Windows**.

### 1.2 EC2 Free Tier Memory Constraint
**Problem:** Running both the Spring Boot backend (JVM) and React frontend (Node.js) simultaneously on a `t2.micro` (1 GB RAM) instance caused OOM (Out of Memory) events and container restarts.

**Solution:** Added explicit JVM heap limits in `docker-compose.yml`:
```yaml
JAVA_TOOL_OPTIONS: "-Xms256m -Xmx512m"
```
This caps JVM heap at 512 MB, leaving sufficient memory for the Node.js frontend and Linux OS processes.

### 1.3 CloudFront Account Verification Requirement
**Problem:** Creating a CloudFront Distribution was blocked by AWS account verification requirements (*"Your account must be verified before you can add new CloudFront resources"*).

**Impact:** CloudFront CDN activation is pending AWS Support verification, though all preparation steps (ACM SSL cert in `us-east-1`, Route 53 DNS validation) are complete. The app remains fully accessible via Route 53 direct routing.

### 1.4 RDS Private Subnet Connectivity
**Problem:** Initial connection attempts from EC2 to RDS PostgreSQL timed out.

**Solution:** Updated RDS Security Group inbound rules to explicitly allow port 5432 from the EC2 Security Group ID.

### 1.5 WebSocket CORS Issues Behind Proxy
**Problem:** SockJS WebSocket connections were rejected with CORS errors when connecting across domains.

**Solution:** Configured Spring Boot `WebSocketConfig` with `setAllowedOriginPatterns("*")` and updated `SecurityConfig` to explicitly allow `https://trackermaint.dpdns.org`.

---

## 2. Development Direction & Future Roadmap

### 2.1 Multi-Region Active-Active Deployment
- Provision EC2 instances in `ap-southeast-1` (Singapore) and `us-east-2` (Ohio).
- Use GitHub Actions Matrix builds to deploy simultaneously across regions.
- Configure AWS Route 53 Latency-Based Routing for automatic geographically nearest server routing.

### 2.2 Amazon CloudFront CDN Integration
- Activate CloudFront Distribution for S3 images and static frontend assets once account verification completes.
- Enable Origin Access Control (OAC) to secure S3 traffic.

### 2.3 Amazon Aurora Global Database
- Migrate from RDS PostgreSQL to Amazon Aurora PostgreSQL using AWS DMS.
- Configure Aurora Global Database with Sydney as Primary and Singapore/Ohio as read replicas.

### 2.4 Infrastructure as Code with AWS CDK
- Replace manual AWS Console setup with TypeScript AWS CDK v2 scripts.
- Single command deployment (`cdk deploy`) for spinning up identical staging/production environments.

### 2.5 Auto Scaling Group with Application Load Balancer
- Replace single EC2 instance with an Auto Scaling Group (ASG) behind an Application Load Balancer (ALB).
- Scale compute instances dynamically based on CPU utilization and incoming traffic spikes.

### 2.6 Advanced Monitoring with CloudWatch Dashboards
- Create custom CloudWatch Dashboards and Alarms to detect brute-force attacks (HTTP 429 spike) and send automated SNS alerts.

### 2.7 Periodic Maintenance Scheduler with AWS EventBridge
- Automate scheduled preventive maintenance reminders using AWS EventBridge Scheduler and Amazon SES / WebSocket notifications.

---

## Summary

This workshop demonstrated a complete end-to-end cloud deployment of a production-ready full-stack web application on AWS. The **Tracker Maintenance System** successfully integrates 10+ AWS services:

- ✅ **Secure Authentication** with JWT + Brute-Force Protection (HTTP 429 lockout)
- ✅ **Cloud Compute** on Amazon EC2 with Docker containerization
- ✅ **Managed Database** on Amazon RDS PostgreSQL (private subnet)
- ✅ **Cloud Storage** on Amazon S3 for equipment images
- ✅ **Real-Time Notifications** via WebSocket (SockJS/STOMP)
- ✅ **QR Code Asset Tracking** with public equipment lookup
- ✅ **Automated CI/CD** via GitHub Actions → ECR → EC2
- ✅ **Cross-Region Image Replication** via ECR Replication Rules
- ✅ **Centralized Log Management** via CloudWatch Logs
- ✅ **Global DNS + SSL** via Route 53 + ACM Certificate (`us-east-1`)
- 🔄 **CloudFront CDN** — Configured, pending AWS account verification

The system is live and accessible at **https://trackermaint.dpdns.org**.