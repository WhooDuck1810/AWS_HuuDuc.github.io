---
title: "Implementation Steps"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3. Implementation Steps

Follow these step-by-step instructions to build the infrastructure, deploy containers, and configure automated pipelines for the **Tracker Maintenance System** on AWS.

---

### Step 5.3.1: Create Custom VPC & Isolated Subnets

1. Open the **Amazon VPC Console** => Click **Create VPC**.
2. Select **VPC only** => Set Name tag to `Tracker-VPC` => Set IPv4 CIDR block to `10.1.0.0/16`.
3. In the navigation pane, choose **Subnets** => Click **Create subnet**:
   - **Public Subnet:** Name `tracker-public-subnet-1`, CIDR `10.1.1.0/24`, Availability Zone `ap-southeast-2a`.
   - **Private Subnet:** Name `tracker-private-subnet-1`, CIDR `10.1.2.0/24`, Availability Zone `ap-southeast-2b`.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS VPC Console screenshot showing the `Tracker-VPC` details and Subnet allocation here.
> 
> ![VPC and Subnet Setup](/images/5-Workshop/5.3-S3-vpc/vpc-subnet-setup.png?classes=shadow)

---

### Step 5.3.2: Configure Internet Gateway & Route Tables

1. In the VPC Console, choose **Internet Gateways** => Click **Create internet gateway** => Name: `tracker-igw`.
2. Select `tracker-igw` => Click **Actions** => Choose **Attach to VPC** => Select `Tracker-VPC`.
3. Choose **Route Tables** => Select the Public Route Table => Click **Edit routes** => Add route: `0.0.0.0/0` targeted to `tracker-igw`.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS Console screenshot showing the Internet Gateway attachment and Route Table configuration here.
> 
> ![Internet Gateway Setup](/images/5-Workshop/5.3-S3-vpc/igw-route-setup.png?classes=shadow)

---

### Step 5.3.3: Configure Virtual Firewalls (Security Groups)

Create two Security Groups inside `Tracker-VPC` to enforce strict network isolation:

1. **EC2 Security Group (`tracker-ec2-sg`):**
   - **Inbound Rules:**
     - SSH (22) => Your IP
     - HTTP (80) => `0.0.0.0/0`
     - HTTPS (443) => `0.0.0.0/0`
     - Custom TCP (3000) => `0.0.0.0/0` (Frontend React)
     - Custom TCP (8081) => `0.0.0.0/0` (Backend Spring Boot API)
2. **RDS Security Group (`tracker-rds-sg`):**
   - **Inbound Rules:**
     - PostgreSQL (5432) => Source: `tracker-ec2-sg` (Security Group ID).

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS Console screenshot showing the Inbound Rules for `tracker-ec2-sg` and `tracker-rds-sg` here.
> 
> ![Security Groups Setup](/images/5-Workshop/5.3-S3-vpc/security-groups-setup.png?classes=shadow)

---

### Step 5.3.4: Provision Amazon RDS PostgreSQL Database

1. Open the **Amazon RDS Console** => Click **Create database**.
2. Choose **Standard create** => Engine: **PostgreSQL** (Version 15.x).
3. Template: **Free Tier** => Instance class: `db.t4g.micro`.
4. Settings: DB instance identifier `tracker-maintenance-db`, Master username `postgres`.
5. Connectivity: Network `Tracker-VPC`, Subnet group in Private Subnet, Public access: **No**.
6. Security group: Choose `tracker-rds-sg`.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS RDS Console screenshot showing the `tracker-maintenance-db` details and Endpoint connection string here.
> 
> ![RDS Database Setup](/images/5-Workshop/5.3-S3-vpc/rds-db-setup.png?classes=shadow)

---

### Step 5.3.5: Configure Amazon S3 Bucket for Media Storage

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

---

### Step 5.3.6: Configure Amazon ECR & Cross-Region Replication

1. Open the **Amazon ECR Console** => Create 2 private repositories: `tracker-be` and `tracker-fe`.
2. Navigate to **Private registry** => **Replication configuration** => Click **Add rule**.
3. Destination regions: Add `ap-southeast-1` (Singapore) and `us-east-2` (Ohio).

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS ECR Console screenshot showing the repositories and Cross-Region Replication rules here.
> 
> ![ECR Setup](/images/5-Workshop/5.3-S3-vpc/ecr-replication-setup.png?classes=shadow)

---

### Step 5.3.7: Launch Amazon EC2 Virtual Server & Elastic IP

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

---

### Step 5.3.8: Configure GitHub Secrets & CI/CD Pipeline

1. In your GitHub repository, navigate to **Settings** => **Secrets and variables** => **Actions**.
2. Add secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `EC2_HOST`, `EC2_SSH_KEY`.
3. Push `.github/workflows/deploy.yml` to the `main` branch to trigger automatic Docker building, ECR pushing, and EC2 deployment.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your GitHub Actions screenshot showing the green successful `Deploy to EC2` workflow run here.
> 
> ![GitHub Actions CI/CD](/images/5-Workshop/5.3-S3-vpc/github-actions-setup.png?classes=shadow)

---

### Step 5.3.9: Configure Route 53 DNS & ACM SSL Certificate

1. Open **Route 53 Console** => Create Hosted Zone for `trackermaint.dpdns.org`.
2. Create an **A Record** pointing `trackermaint.dpdns.org` to the EC2 Elastic IP (`3.106.194.112`).
3. Request an **ACM Certificate** in `us-east-1` (N. Virginia) for `trackermaint.dpdns.org` to prepare for CloudFront integration.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS Route 53 Console screenshot showing the A Record details here.
> 
> ![Route 53 Setup](/images/5-Workshop/5.3-S3-vpc/route53-acm-setup.png?classes=shadow)
