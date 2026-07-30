---
title: "Configure Amazon S3 Bucket for Media Storage"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1 Configure Amazon S3 Bucket for Media Storage

In this step, create an Amazon S3 Bucket to store uploaded equipment photos and maintenance evidence offloaded from EC2.

### Bucket Policy Configuration
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

<div style="text-align: center; margin: 20px 0;">

  ![S3 Bucket Policy](/images/5-Workshop/5.4-S3-onprem/5.4.1-s3-bucket/s3-policy.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.4.1. Amazon S3 Bucket Permissions showing Block Public Access settings and PublicReadGetObject policy.</div>
</div>
