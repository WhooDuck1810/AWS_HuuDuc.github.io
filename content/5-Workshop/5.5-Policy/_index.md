---
title: "Clean Up"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. Clean Up

When the workshop is complete or resources are no longer needed, follow these steps to avoid ongoing AWS charges:

---

### Step 5.5.1: Stop (Not Terminate) EC2 Instance

If you want to preserve your setup for later use:
1. Navigate to **EC2 Console** => **Instances**.
2. Select `tracker-ec2-server` => Click **Instance State** => **Stop instance**.
3. A stopped EC2 instance incurs **no compute charges**, only minimal EBS storage costs (~$0.08/GB/month).

---

### Step 5.5.2: Stop RDS Database Instance

1. Navigate to **RDS Console** => **Databases**.
2. Select `tracker-maintenance-db` => Click **Actions** => **Stop temporarily**.
3. RDS can be stopped for up to 7 days. During the stopped period, you pay only for database storage.

---

### Step 5.5.3: Delete S3 Objects (Optional)

To avoid storage charges for uploaded images:
1. Navigate to **S3 Console** => `tracker-maintenance-images-123`.
2. Select all objects => Click **Delete**.
3. The empty bucket itself costs nothing.

---

### Step 5.5.4: Delete ECR Images (Optional)

1. Navigate to **ECR Console** => **Private Repositories**.
2. Select `tracker-be` => Delete `:latest` tag.
3. Repeat for `tracker-fe`.

---

### Step 5.5.5: Delete CloudWatch Log Groups (Optional)

1. Navigate to **CloudWatch Console** => **Log Groups**.
2. Select `/tracker-maintenance/backend` and `/tracker-maintenance/frontend` => **Actions** => **Delete log group**.

---

### Step 5.5.6: Permanent Resource Termination (End of Internship)

If permanently decommissioning the environment:
1. **Terminate** the EC2 instance.
2. **Delete** the RDS PostgreSQL database.
3. **Delete** the S3 bucket (empty objects first).
4. **Delete** ECR repositories.
5. **Delete** ACM SSL certificate in `us-east-1`.
6. **Delete** Route 53 hosted zone.
7. **Revoke** IAM User access keys & Delete IAM Role.
