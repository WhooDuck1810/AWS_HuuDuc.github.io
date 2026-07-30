---
title: "Configure Amazon ECR & Cross-Region Replication"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2 Configure Amazon ECR & Cross-Region Replication

In this step, create private Docker registries on Amazon ECR for backend (`tracker-be`) and frontend (`tracker-fe`) images.

> [!NOTE]
> ℹ️ **Important Note regarding Repository List:** In the screenshot below, the repository `aws/tracker_maintenance_app` was an initial experimental testing repository. Please focus exclusively on the two production repositories: **`tracker-be`** and **`tracker-fe`**.

<div style="text-align: center; margin: 20px 0;">

  ![ECR Repositories](/images/5-Workshop/5.4-S3-onprem/5.4.2-ecr-replication/ecr-repositories.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.4.2. Amazon ECR Private Repositories list displaying tracker-be and tracker-fe container images.</div>
</div>
