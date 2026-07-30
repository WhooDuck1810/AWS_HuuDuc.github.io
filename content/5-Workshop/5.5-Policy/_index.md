---
title: "Clean Up"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5. Clean Up

When the workshop is complete or resources are no longer needed, follow these steps to avoid ongoing AWS charges:

## 5.1 Stop (Not Terminate) EC2 Instance

If you want to preserve your setup for later use:
1. Go to **EC2 → Instances** in the AWS Console.
2. Select the EC2 instance → **Instance State → Stop**.
3. A stopped EC2 instance incurs **no compute charges**, only EBS storage costs (~$0.08/GB/month).

## 5.2 Stop RDS Instance

1. Go to **RDS → Databases**.
2. Select the `tracker-maintenance-db` instance → **Actions → Stop temporarily**.
3. RDS can be stopped for up to 7 days. After 7 days, AWS automatically restarts it.
4. During the stopped period, you pay only for storage (no instance charges).

## 5.3 Delete S3 Objects (Optional)

To avoid storage charges for uploaded images:
1. Go to **S3 → tracker-maintenance-images-123**.
2. Select all objects → **Delete**.
3. The empty bucket itself costs nothing.

## 5.4 Delete ECR Images (Optional)

1. Go to **ECR → Private Repositories**.
2. Select `tracker-be` → **Images** → Select the `:latest` image → **Delete**.
3. Repeat for `tracker-fe`.
4. ECR charges $0.10/GB/month for stored image data.

## 5.5 Delete CloudWatch Log Groups (Optional)

1. Go to **CloudWatch → Log Groups**.
2. Select `/tracker-maintenance/backend` and `/tracker-maintenance/frontend` → **Actions → Delete**.
3. CloudWatch Logs are charged at $0.50/GB ingested and $0.03/GB/month stored.

## 5.6 Preserve for Full Termination (End of Internship)

If permanently shutting down:
1. **Terminate** (not just stop) the EC2 instance.
2. **Delete** the RDS database (create a final snapshot if you want to keep the data).
3. **Delete** the S3 bucket (empty it first, then delete).
4. **Delete** the ECR repositories.
5. **Delete** the ACM certificate in `us-east-1`.
6. **Delete** the Route 53 hosted zone.
7. **Revoke** IAM user access keys for GitHub Actions.
8. **Delete** the IAM role attached to EC2.