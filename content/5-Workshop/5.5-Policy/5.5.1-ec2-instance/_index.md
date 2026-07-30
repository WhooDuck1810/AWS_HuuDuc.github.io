---
title: "Provision EC2 Instance & Elastic IP"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1 Provision EC2 Instance & Elastic IP

In this step, launch the Amazon EC2 virtual server and attach a static Elastic IP address.

1. Open the **Amazon EC2 Console** => Click **Launch instance**.
2. Name: `tracker-ec2-server`, AMI: **Amazon Linux 2023**, Instance type: `t2.micro`.
3. Network settings: VPC `Tracker-VPC`, Subnet `tracker-public-subnet-1`, Security group `tracker-ec2-sg`.
4. Advanced details: IAM instance profile => Select `tracker-ec2-role`.
5. Allocate an **Elastic IP** and associate it with the EC2 instance (`3.106.194.112`).
6. SSH into EC2 and install Docker & Docker Compose:
   ```bash
   sudo yum install docker -y
   sudo systemctl enable docker && sudo systemctl start docker
   sudo usermod -aG docker ec2-user
   ```

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS EC2 Console screenshot showing the `tracker-ec2-server` status and Elastic IP association here.
> 
> ![EC2 Instance Setup](/images/5-Workshop/5.3-S3-vpc/ec2-instance-setup.png?classes=shadow)
