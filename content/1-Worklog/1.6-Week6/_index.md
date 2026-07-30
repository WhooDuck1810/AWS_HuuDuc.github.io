---
title: "Week 6 - AWS Monitoring CloudWatch & IaC CloudFormation"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
url: "/en/1-worklog/1.6-week6/"
---

### Weekly Topic

System Metrics Monitoring + Infrastructure as Code Provisioning

### Weekly Objectives

* Collect logs and metrics to monitor resource health status.
* Transition from manual configuration tasks to Infrastructure as Code scripts.

### Work Schedule

| Date | Day | Task Description | Reference Links |
| :--- | :--- | :--- | :--- |
| 06/07/2026 | Monday | - Begin Amazon CloudWatch monitoring module<br>- Differentiate Metrics, Alarms, and Logs<br>- Learn about the SNS notification service | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ |
| 07/07/2026 | Tuesday | - Practice resource monitoring:<br>&emsp;+ View EC2 server CPU metrics<br>&emsp;+ Set an alarm to send emails when CPU > 80%<br>&emsp;+ Run a stress test script to trigger the alarm | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html |
| 08/07/2026 | Wednesday | - Infrastructure as Code (IaC) concept<br>- Explore CloudFormation Template structure:<br>&emsp;+ Parameters<br>&emsp;+ Resources<br>&emsp;+ Outputs | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/ |
| 09/07/2026 | Thursday | - Practice infrastructure scripting:<br>&emsp;+ Script YAML format to create VPC and Subnets<br>&emsp;+ Upload file to CloudFormation Console to deploy a Stack<br>&emsp;+ Verify results on the VPC Dashboard | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html |
| 10/07/2026 | Friday | - Clean up (Delete Stack) to automatically reclaim all resources<br>- Record week 6 practice log | https://cloudjourney.awsstudygroup.com/ |

### Expected Outcomes

* Monitoring system actively alarms upon signs of metric anomalies.
* Resource creation processes are standardized into reusable configuration files.

### Week 6 References

* Amazon CloudWatch Guide: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/
* AWS CloudFormation Guide: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/
