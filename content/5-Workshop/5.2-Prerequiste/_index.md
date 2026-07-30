---
title: "Prerequisite"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 2. Prerequisite

## 2.1 Knowledge Prerequisites

Before beginning this workshop, participants should have a working understanding of:

- **Basic Linux Command Line:** Ability to SSH into a server, navigate directories, and run shell commands.
- **Docker Fundamentals:** Understanding of what Docker images and containers are, how `docker-compose.yml` works, and basic commands (`docker pull`, `docker ps`, `docker logs`).
- **REST API Concepts:** Understanding of HTTP methods (GET, POST, PUT, DELETE), HTTP status codes, and JSON request/response format.
- **Basic AWS Console Navigation:** Ability to log into the AWS Management Console, switch regions, and navigate between services.
- **Git Basics:** Ability to `git commit`, `git push`, and understand GitHub repository concepts.
- **Spring Boot Basics (Optional):** Familiarity with Spring MVC controller structure, `@RestController`, `@Service`, and `@Repository` patterns.
- **ReactJS Basics (Optional):** Familiarity with React components, hooks (`useState`, `useEffect`, `useContext`), and routing.

## 2.2 Infrastructure Prerequisites

The following AWS resources must be provisioned **before** beginning the implementation steps:

### 2.2.1 AWS Account
- An active **AWS Account** with root or IAM Administrator access.
- The account must have access to the following services: EC2, RDS, S3, ECR, CloudWatch, IAM, Route 53, CloudFront, ACM.

### 2.2.2 Amazon EC2 Instance
- **Instance Type:** `t2.micro` (1 vCPU, 1 GB RAM) — eligible for AWS Free Tier.
- **Region:** `ap-southeast-2` (Sydney, Australia).
- **AMI:** Amazon Linux 2023.
- **Storage:** 20 GB gp3 EBS volume.
- **Key Pair:** A `.pem` key pair saved locally for SSH access (e.g., `tracker-key.pem`).
- **Security Group (Inbound Rules):**

| Port | Protocol | Source | Purpose |
|---|---|---|---|
| 22 | TCP | Your IP (or GitHub Actions IP) | SSH access |
| 80 | TCP | 0.0.0.0/0 | HTTP web traffic |
| 443 | TCP | 0.0.0.0/0 | HTTPS web traffic |
| 3000 | TCP | 0.0.0.0/0 | Frontend React app |
| 8081 | TCP | 0.0.0.0/0 | Backend Spring Boot API |

- **Software pre-installed on EC2:**
  - Docker Engine (`sudo yum install docker -y && sudo systemctl enable docker && sudo systemctl start docker`)
  - Docker Compose plugin (`sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose`)
  - AWS CLI v2 (pre-installed on Amazon Linux 2023)

### 2.2.3 Amazon RDS PostgreSQL Instance
- **Engine:** PostgreSQL 15.
- **Instance Class:** `db.t4g.micro` (2 vCPU, 1 GB RAM).
- **Region:** `ap-southeast-2` (Sydney).
- **Multi-AZ:** Disabled (Single-AZ for cost optimization).
- **Public Accessibility:** Disabled (private subnet only, accessible from EC2 within the same VPC).
- **Database name:** `postgres`
- **Master username:** `postgres`
- **Endpoint:** `tracker-maintenance-db.cvow26so4q44.ap-southeast-2.rds.amazonaws.com:5432`
- **Security Group:** Inbound rule allowing port 5432 (PostgreSQL) from the EC2 Security Group.

### 2.2.4 Amazon S3 Bucket
- **Bucket Name:** `tracker-maintenance-images-123`
- **Region:** `ap-southeast-2` (Sydney).
- **Public Access Settings:** Block all public access = **Disabled** (to allow public image URL reading).
- **Bucket Policy:** Allow public `s3:GetObject` on all objects.
- **CORS Configuration:** Allow `PUT` and `GET` from the application domain.

### 2.2.5 Amazon ECR Repositories
Two private ECR repositories in `ap-southeast-2`:
- `tracker-be` — stores the Spring Boot backend Docker image.
- `tracker-fe` — stores the React frontend Docker image.

### 2.2.6 IAM Setup
- **IAM User for GitHub Actions CI/CD:**
  - Programmatic access (Access Key ID + Secret Access Key).
  - Permissions: `AmazonEC2ContainerRegistryFullAccess` (to push Docker images to ECR).
  - Store as GitHub Repository Secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`.
- **IAM Role for EC2:**
  - Attached to the EC2 instance.
  - Permissions: `AmazonEC2ContainerRegistryReadOnly` (to pull Docker images from ECR), `CloudWatchAgentServerPolicy` (to push logs to CloudWatch).

### 2.2.7 Domain and DNS
- **Domain:** `trackermaint.dpdns.org` — registered and managed via DuckDNS or a similar free DNS provider.
- **Route 53 Hosted Zone:** The domain is configured in AWS Route 53 with an `A Record` pointing to the EC2 Elastic IP.
- **ACM SSL Certificate:** Provisioned for `trackermaint.dpdns.org` in `us-east-1 (N. Virginia)` for CloudFront compatibility.

### 2.2.8 Local Developer Environment
- **Operating System:** Windows 10/11.
- **JDK:** Java 21 (Amazon Corretto or Temurin).
- **Node.js:** v18 or v20 LTS.
- **Maven Wrapper:** `mvnw.cmd` included in the Spring Boot project.
- **Docker Desktop:** Installed locally for testing container builds.
- **Git:** Installed and configured with SSH key to GitHub.
- **SSH Key:** `C:\Users\Duc\Downloads\key\tracker-key.pem` for EC2 access.
- **IDE:** IntelliJ IDEA (backend) and Visual Studio Code (frontend).

### 2.2.9 GitHub Repository and Secrets

GitHub Repository: `https://github.com/WhooDuck1810/TrackerMaintenance`

Required GitHub Repository Secrets:
| Secret Name | Description |
|---|---|
| `AWS_ACCESS_KEY_ID` | IAM User Access Key for ECR push |
| `AWS_SECRET_ACCESS_KEY` | IAM User Secret Key for ECR push |
| `EC2_HOST` | Public IP of the EC2 instance (`3.106.194.112`) |
| `EC2_SSH_KEY` | Content of the `.pem` private key file for EC2 SSH |
| `APP_R2_ACCESS_KEY_ID` | AWS Access Key for S3 image upload |
| `APP_R2_SECRET_ACCESS_KEY` | AWS Secret Key for S3 image upload |
