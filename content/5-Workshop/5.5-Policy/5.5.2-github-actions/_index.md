---
title: "Configure GitHub Secrets & CI/CD Pipeline"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2 Configure GitHub Secrets & CI/CD Pipeline

In this step, configure repository secrets and the automated deployment workflow.

1. In your GitHub repository, navigate to **Settings** => **Secrets and variables** => **Actions**.
2. Add secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `EC2_HOST`, `EC2_SSH_KEY`.
3. Push `.github/workflows/deploy.yml` to the `main` branch to trigger automatic Docker building, ECR pushing, and EC2 deployment.

```yaml
name: Deploy to EC2 (Tracker Maintenance)
on:
  push:
    branches:
      - main
jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-southeast-2
      - run: |
          docker build -t 534120921488.dkr.ecr.ap-southeast-2.amazonaws.com/tracker-be:latest ./tracker_maintenance_service
          docker push 534120921488.dkr.ecr.ap-southeast-2.amazonaws.com/tracker-be:latest
  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ec2-user
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd /home/ec2-user
            docker-compose pull && docker-compose up -d
```

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your GitHub Actions screenshot showing the green successful `Deploy to EC2` workflow run here.
> 
> ![GitHub Actions CI/CD](/images/5-Workshop/5.3-S3-vpc/github-actions-setup.png?classes=shadow)
