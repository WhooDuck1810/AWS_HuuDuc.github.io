---
title: "Test Results & Experimentation"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. Test Results & Experimentation

After completing the infrastructure deployment and CI/CD setup, execute empirical tests to verify system functionality, security controls, and real-time operations.

---

### Step 5.4.1: Unit Tests — LoginAttemptService Verification

Run backend unit tests to verify in-memory rate limiting:
```bash
.\mvnw.cmd test
```

| Test Case | Description | Result |
|---|---|---|
| `testUsernameNotBlockedInitially()` | Fresh user with 0 attempts is not blocked | ✅ PASS |
| `testUsernameBlockedAfterMaxAttempts()` | Account locked after 5 failed attempts | ✅ PASS |
| `testIpBlockedAfterMaxAttempts()` | IP locked after 20 failed attempts | ✅ PASS |
| `testSuccessfulLoginResetsAttempts()` | Successful login resets counter | ✅ PASS |
| `testUnblockAfterLockoutPeriod()` | Lockout expires after 15 minutes | ✅ PASS |

---

### Step 5.4.2: Brute-Force Protection — Manual Lockout Test

Simulate an automated password guessing attack against `/auth/token`:

| Attempt | Password | HTTP Response Code |
|---|---|---|
| 1st – 5th | `wrongpass` | `401 Unauthorized` |
| 6th | `wrongpass` | `429 Too Many Requests` (Account Locked) |
| 7th | `Admin123!` (Correct) | `429 Too Many Requests` (Lockout Preserved) |

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your browser/Postman screenshot showing the HTTP 429 Too Many Requests response here.
> 
> ![Brute Force Lockout Test](/images/5-Workshop/5.4-S3-onprem/brute-force-test.png?classes=shadow)

---

### Step 5.4.3: Real-Time WebSocket Notification Test

1. Open Manager Dashboard in Browser Tab 1 and Technician View in Browser Tab 2.
2. Manager creates and assigns a maintenance ticket to the Technician.

**Results:**
- Tab 2 receives a real-time toast notification without page refresh.
- Browser tab title updates to `(1) My Tickets | Tracker Maintenance`.
- Notification bell displays a red badge count `1`. ✅

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your web application screenshot showing the real-time WebSocket toast and browser tab title update here.
> 
> ![WebSocket Notification Test](/images/5-Workshop/5.4-S3-onprem/websocket-test.png?classes=shadow)

---

### Step 5.4.4: S3 Image Upload & QR Code Scan Test

1. Upload an equipment photo from the Equipment Detail page.
2. Scan the generated QR code using a smartphone.

**Results:**
- Image is uploaded directly to S3 and renders via `https://tracker-maintenance-images-123.s3.ap-southeast-2.amazonaws.com/equipment/eq-001/photo.jpg`.
- Smartphone camera opens `https://trackermaint.dpdns.org/public/equipment/1` displaying full equipment details without requiring login. ✅

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your web application screenshot showing the QR Code display and S3 uploaded photo here.
> 
> ![S3 and QR Code Test](/images/5-Workshop/5.4-S3-onprem/qrcode-s3-test.png?classes=shadow)

---

### Step 5.4.5: CloudWatch Centralized Logging Verification

Verify that application container logs stream to AWS CloudWatch Logs in real time:
- Backend Log Group: `/tracker-maintenance/backend`
- Frontend Log Group: `/tracker-maintenance/frontend`

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your AWS CloudWatch Logs Console screenshot showing live container log streams here.
> 
> ![CloudWatch Logs Test](/images/5-Workshop/5.4-S3-onprem/cloudwatch-logs-test.png?classes=shadow)
