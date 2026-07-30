---
title: "Brute-Force Anti-Login Protection"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3 Brute-Force Anti-Login Protection Mechanism (HTTP 429)

In this step, verify the in-memory rate limiting and lockout mechanism protecting `/auth/token`.

- **Logic:** `LoginAttemptService` tracks failed attempts per username and per IP using thread-safe `ConcurrentHashMap`.
- **Lockout Rule:** Lock account after 5 consecutive failures for 15 minutes (`HTTP 429 Too Many Requests`).

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your browser/Postman screenshot showing the HTTP 429 response here.
> 
> ![Brute Force Protection](/images/5-Workshop/5.4-S3-onprem/brute-force-test.png?classes=shadow)
