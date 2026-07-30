---
title: "Centralized Logging & Monitoring with CloudWatch"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

# 5.6.2 Centralized Logging & Monitoring with CloudWatch

In this step, inspect real-time database metrics and system log groups in Amazon CloudWatch.

### 1. Database Insights & Infrastructure Monitoring
- **Amazon CloudWatch Database Insights** automatically collects CPU utilization, active database sessions, and top SQL execution statements for `tracker-maintenance-db`.
- Database Load Utilization: `0.1%` (Active monitoring without alarms).

<div style="text-align: center; margin: 20px 0;">

  ![CloudWatch Database Insights](/images/5-Workshop/5.6-Cleanup/5.6.2-cloudwatch-logs/cloudwatch-db-insights.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.6.3. CloudWatch Database Insights dashboard monitoring tracker-maintenance-db active sessions, CPU load, and Top SQL statements.</div>
</div>

### 2. CloudWatch Log Management & Log Groups
- **`RDSOSMetrics`**: Automatically streams OS-level metrics and telemetry for the PostgreSQL database (1-month retention).
- **Custom Application Logs**: Streamed directly from container standard output to CloudWatch Log Groups.

<div style="text-align: center; margin: 20px 0;">

  ![CloudWatch Log Groups](/images/5-Workshop/5.6-Cleanup/5.6.2-cloudwatch-logs/cloudwatch-log-groups.png?classes=shadow)

  <div style="font-weight: bold; margin-top: 8px; color: #555;">Figure 5.6.4. CloudWatch Log Groups management dashboard displaying RDSOSMetrics telemetry stream.</div>
</div>
