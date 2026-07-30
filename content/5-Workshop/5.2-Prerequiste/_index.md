---
title: "Prerequisite"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. Prerequisite

Before beginning the deployment of the Tracker Maintenance System on AWS, prepare the local developer workstation and IAM access credentials.

---

### Step 5.2.1: Workstation & Local Environment Preparation

1. **Install JDK 21 (Corretto or Temurin)** and **Node.js v20 LTS** on your developer workstation.
2. **Install Docker Desktop & Git** for local testing and source code version control.
3. Prepare local `.env` variables and ensure sensitive credentials (`AWS_ACCESS_KEY_ID`, `JWT_SECRET`, database passwords) are added to `.gitignore`.

---

### Step 5.2.2: Create IAM User for GitHub Actions CI/CD Pipeline

To enable automated Docker image builds and deployment via GitHub Actions without hardcoding root account credentials:

1. Navigate to the **AWS IAM Console** => **Users** => Click **Create user**.
2. Set the username to `github-actions-deployer`.
3. Under **Permissions options**, choose **Attach policies directly**.
4. Search for and select `AmazonEC2ContainerRegistryFullAccess`.
5. Confirm creation, then navigate to **Security credentials** => **Create access key** => Choose **Command Line Interface (CLI)**.
6. Copy and save the generated **Access Key ID** and **Secret Access Key** securely.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS IAM Console screenshot showing the `github-actions-deployer` user details and attached ECR policy here.
> 
> ![IAM User Setup](/images/5-Workshop/5.2-Prerequisite/iam-user-setup.png?classes=shadow)

---

### Step 5.2.3: Create IAM Role for EC2 Virtual Server

To grant the EC2 instance permission to pull Docker images from Amazon ECR and push logs to CloudWatch without storing static credentials on the server:

1. Navigate to **IAM Console** => **Roles** => Click **Create role**.
2. Select **AWS Service** as the trusted entity type, and choose **EC2** as the use case.
3. Attach the following two AWS managed policies:
   - `AmazonEC2ContainerRegistryReadOnly`
   - `CloudWatchAgentServerPolicy`
4. Name the role `tracker-ec2-role` and click **Create role**.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS IAM Console screenshot showing the `tracker-ec2-role` with attached policies here.
> 
> ![IAM Role Setup](/images/5-Workshop/5.2-Prerequisite/iam-role-setup.png?classes=shadow)

---

### Step 5.2.4: Why Use IAM Accounts Instead of the Root Account?

- **Principle of Least Privilege (PoLP):** Grants only the exact permissions needed for specific automated pipelines or compute instances.
- **Root Credentials Security:** Prevents accidental leakage of root credentials in code repositories or CI/CD settings.
- **Auditability & Compliance:** AWS CloudTrail logs every API action with the exact IAM User/Role identity.

