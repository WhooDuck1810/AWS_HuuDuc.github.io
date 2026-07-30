---
title: "Test Results & Experimentation"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 4. Test Results & Experimentation

## 4.1 Unit Tests — LoginAttemptService

**Test File:** `tracker_maintenance_service/src/test/java/.../service/LoginAttemptServiceTest.java`

All 5 unit tests pass with a 100% success rate when executing `.\mvnw.cmd test`:

| Test Case | Description | Result |
|---|---|---|
| `testUsernameNotBlockedInitially()` | A fresh username with no attempts should not be blocked | ✅ PASS |
| `testUsernameBlockedAfterMaxAttempts()` | After 5 failed attempts, the username should be blocked | ✅ PASS |
| `testIpBlockedAfterMaxAttempts()` | After 20 failed attempts from the same IP, the IP should be blocked | ✅ PASS |
| `testSuccessfulLoginResetsAttempts()` | A successful login should reset the failed attempt counter | ✅ PASS |
| `testUnblockAfterLockoutPeriod()` | After the 15-minute lockout window expires, the account should be unblocked | ✅ PASS |

**Test Output:**
```
[INFO] Tests run: 5, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

## 4.2 Brute-Force Protection — Manual Testing

**Test Scenario:** Attempt to log in with an incorrect password 6 times in succession.

| Attempt | Username | Password | HTTP Response |
|---|---|---|---|
| 1st | admin | wrongpass | `401 Unauthorized` — Invalid credentials |
| 2nd | admin | wrongpass | `401 Unauthorized` — Invalid credentials |
| 3rd | admin | wrongpass | `401 Unauthorized` — Invalid credentials |
| 4th | admin | wrongpass | `401 Unauthorized` — Invalid credentials |
| 5th | admin | wrongpass | `401 Unauthorized` — Invalid credentials |
| 6th | admin | wrongpass | `429 Too Many Requests` — **Account Locked** |
| 7th (correct) | admin | Admin123! | `429 Too Many Requests` — **Still locked** |

**Result:** The account is correctly locked after 5 consecutive failures, and even a correct password cannot bypass the lockout during the 15-minute window. ✅

## 4.3 CI/CD Pipeline — Deployment Test

**Test:** Push a trivial code change to the `main` branch and observe the full automated deployment.

**GitHub Actions Run Timeline:**
```
00:00 - Push commit to main
00:02 - GitHub Actions workflow triggered
00:15 - AWS credentials configured
00:30 - Docker build (Backend) started
02:30 - Backend JAR built (Maven compile + package)
03:00 - Backend Docker image pushed to ECR ap-southeast-2
03:05 - Docker build (Frontend) started
04:00 - Frontend React build completed
04:20 - Frontend Docker image pushed to ECR ap-southeast-2
04:25 - SSH connection established to EC2 (3.106.194.112)
04:35 - ECR login on EC2 successful
04:40 - docker-compose pull completed (new images downloaded)
04:55 - docker-compose up -d completed (containers restarted)
05:00 - Old images pruned
05:05 - Workflow SUCCESS ✅
```

**Total deployment time:** ~5 minutes from `git push` to live production update. ✅

## 4.4 Real-Time Notification — WebSocket Test

**Test Scenario:**
1. Open Browser Tab 1 (logged in as MANAGER) and Browser Tab 2 (logged in as TECHNICIAN).
2. Manager creates a new ticket and assigns it to the Technician.

**Actual Result:**
- Tab 2 (Technician) shows a toast notification in real time without refreshing the page.
- Tab 2 title updates from `My Tickets | Tracker Maintenance` to `(1) My Tickets | Tracker Maintenance`.
- The notification bell icon shows a red badge with count `1`. ✅

## 4.5 S3 Image Upload — Test

**Test:** Upload a PNG equipment photo from the Equipment Detail page.

| Step | Action | Result |
|---|---|---|
| 1 | Click "Upload Image" button on equipment `EQ-001` | File picker opens |
| 2 | Select a 2 MB JPEG photo | File selected |
| 3 | Click "Upload" | Loading spinner shows |
| 4 | After 1.2 seconds | Success toast: "Image uploaded successfully" |
| 5 | Page refreshes equipment image | Image loads from S3 URL |
| 6 | Open S3 bucket console | File visible in `equipment/eq-001/` prefix |

**S3 Public URL Format:** `https://tracker-maintenance-images-123.s3.ap-southeast-2.amazonaws.com/equipment/eq-001/photo-1234567890.jpg` ✅

## 4.6 QR Code Scan — Test

**Test:** Scan the QR code displayed on the Equipment Detail page using a smartphone.

**Result:** Smartphone camera recognized the QR code, browser opened `https://trackermaint.dpdns.org/public/equipment/1`, and displayed full equipment information including status, model, serial number, location, and maintenance history — all without requiring login. ✅

## 4.7 CloudWatch Logs — Test

- CloudWatch Log Group `/tracker-maintenance/backend` appeared automatically within 30 seconds of the first container startup.
- All Spring Boot startup logs, SQL queries, and HTTP request logs were visible in real time in the CloudWatch Logs console. ✅

## 4.8 ECR Cross-Region Replication — Test

**Test:** After configuring ECR replication rules, push a new Docker image to ECR Sydney.

**Result:** Within 60 seconds of the push completing, both `tracker-be` and `tracker-fe` repositories (with the `:latest` tag) appeared automatically in ECR `ap-southeast-1` (Singapore) without any manual intervention. ✅