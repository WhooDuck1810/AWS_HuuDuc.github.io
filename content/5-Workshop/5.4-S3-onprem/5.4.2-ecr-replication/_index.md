---
title: "Configure Amazon ECR & Cross-Region Replication"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2 Configure Amazon ECR & Cross-Region Replication

In this step, create private Docker repositories in Sydney and configure automatic replication to Singapore and Ohio.

1. Open the **Amazon ECR Console** => Create 2 private repositories: `tracker-be` and `tracker-fe`.
2. Navigate to **Private registry** => **Replication configuration** => Click **Add rule**.
3. Destination regions: Add `ap-southeast-1` (Singapore) and `us-east-2` (Ohio).

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS ECR Console screenshot showing the repositories and Cross-Region Replication rules here.
> 
> ![ECR Setup](/images/5-Workshop/5.3-S3-vpc/ecr-replication-setup.png?classes=shadow)
