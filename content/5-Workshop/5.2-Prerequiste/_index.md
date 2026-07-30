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

### Step 5.2.2: Create IAM User (`tracker-s3-uploader-2`) for Programmatic S3 & Deployment Access

To enable programmatic media uploads from the Spring Boot backend to Amazon S3 and automated deployment without using the AWS Root account or `admin1` user:

1. Navigate to the **AWS IAM Console** => **Users** => Click **Create user**.
2. Set the username to `tracker-s3-uploader-2`.
3. Under **Permissions options**, choose **Attach policies directly**.
4. Search for and select policies for S3 storage access (e.g. `AmazonS3FullAccess` or custom S3 upload policy) and ECR access (`AmazonEC2ContainerRegistryFullAccess`).
5. Confirm creation, then navigate to **Security credentials** => **Create access key** => Choose **Command Line Interface (CLI)**.
6. Copy and save the generated **Access Key ID** and **Secret Access Key** securely into your `.env` and GitHub Secrets.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS IAM Console screenshot showing the `tracker-s3-uploader-2` user details and permissions here.
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

### Step 5.2.4: Why Use IAM Accounts (`tracker-s3-uploader-2`) Instead of the Root Account or `admin1`?

- **Principle of Least Privilege (PoLP):** Isolates S3 uploading and deployment permissions specifically to `tracker-s3-uploader-2` rather than exposing full administrative permissions (`admin1` / Root).
- **Security Isolation:** If an application credential is ever compromised, the blast radius is strictly limited to S3/ECR operations, protecting overall AWS account administration.
- **Auditability & Compliance:** AWS CloudTrail logs every API action with the exact `tracker-s3-uploader-2` identity.
