---
title: "Week 8 - Project Provisioning & Setup CI/CD Pipeline"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
url: "/en/1-worklog/1.8-week8/"
---

### Weekly Topic

Cloud Resource Provisioning + Continuous Integration and Deployment

### Weekly Objectives

* Transform the architecture diagram into practical infrastructure on the cloud platform.
* Set up a continuous deployment pipeline to automate moving source code to environments.

### Work Schedule

| Date | Day | Task Description | Reference Links |
| :--- | :--- | :--- | :--- |
| 20/07/2026 | Monday | - Provision networking foundation resources:<br>&emsp;+ Initialize VPC, Internet Gateway, NAT Gateway<br>&emsp;+ Accurately configure Route Tables<br>&emsp;+ Set up Security Groups | Week 7 Architecture Diagram |
| 21/07/2026 | Tuesday | - Deploy compute services:<br>&emsp;+ Install OS on EC2 servers<br>&emsp;+ Configure Elastic Load Balancers (ELB) if required<br>&emsp;+ Push Backend source code to servers | Backend Deployment Guide |
| 22/07/2026 | Wednesday | - Deploy data and static storage:<br>&emsp;+ Initialize and connect RDS/DynamoDB databases<br>&emsp;+ Upload static Frontend code to S3 bucket<br>&emsp;+ Link API between Frontend and Backend | DB Deployment Guide |
| 23/07/2026 | Thursday | - Configure CI/CD pipelines:<br>&emsp;+ Connect source code from GitHub to CodePipeline<br>&emsp;+ Define automated build processes (CodeBuild)<br>&emsp;+ Set up automated deployment processes (CodeDeploy) | https://docs.aws.amazon.com/codepipeline/latest/userguide/ |
| 24/07/2026 | Friday | - Verify seamless operation of application Endpoints<br>- Handle initial connectivity bugs<br>- Record week 8 progress | https://cloudjourney.awsstudygroup.com/ |

### Expected Outcomes

* Entire infrastructure architecture is fully provisioned and ready to serve network traffic.
* Deployment pipeline operates stably, automatically compiling and distributing source code.

### Week 8 References

* AWS CodePipeline Guide: https://docs.aws.amazon.com/codepipeline/latest/userguide/
* DevOps on AWS Practices: https://aws.amazon.com/devops/
