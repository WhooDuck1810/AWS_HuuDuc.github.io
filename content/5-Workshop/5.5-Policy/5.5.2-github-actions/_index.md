---
title: "Configure GitHub Secrets & CI/CD Pipeline"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2 Configure GitHub Secrets & CI/CD Pipeline

In this step, store AWS authentication credentials in GitHub Repository Secrets and execute the automated CI/CD pipeline.

### 1. Repository Secrets Configuration
In GitHub Repository => **Settings** => **Secrets and variables** => **Actions**, add:
- `AWS_ACCESS_KEY_ID`: Access key for IAM user `tracker-s3-uploader-2`
- `AWS_SECRET_ACCESS_KEY`: Secret access key for IAM user `tracker-s3-uploader-2`
- `EC2_HOST`: Elastic IP of EC2 server (`3.106.194.112`)
- `EC2_SSH_KEY`: SSH Private Key for `ec2-user`

### 2. Automated Workflow Runs
Every git push to the `main` branch automatically triggers the Docker container build, ECR push, and SSH deployment to EC2.

<div style="text-align: center; margin: 20px 0;">

  ![GitHub Actions Runs](/images/5-Workshop/5.5-Policy/5.5.2-github-actions/github-actions-runs.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.5.2. GitHub Actions automated workflow execution log showing successful Deploy to EC2 runs.</div>
</div>
