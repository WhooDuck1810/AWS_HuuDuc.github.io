---
title: "Configure Amazon S3 Bucket for Media Storage"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1 Configure Amazon S3 Bucket for Media Storage

In this step, create an Amazon S3 Bucket to store uploaded equipment photos and maintenance evidence offloaded from EC2.

1. Open the **Amazon S3 Console** => Click **Create bucket**.
2. Bucket name: `tracker-maintenance-images-123`, Region: `ap-southeast-2` (Sydney).
3. Object Ownership: ACLs disabled => **Block Public Access settings:** Uncheck *Block all public access* (Disable).
4. Save creation, then open **Permissions** tab => Add **Bucket Policy**:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::tracker-maintenance-images-123/*"
       }
     ]
   }
   ```
5. Configure **CORS** rules to allow `PUT` and `GET` from the web application domain.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS S3 Console screenshot showing the bucket overview and Block Public Access status here.
> 
> ![S3 Bucket Setup](/images/5-Workshop/5.3-S3-vpc/s3-bucket-setup.png?classes=shadow)
