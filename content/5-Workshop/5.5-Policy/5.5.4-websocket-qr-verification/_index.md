---
title: "Real-Time WebSocket & QR Code Asset Tracking"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

# 5.5.4 Real-Time WebSocket & QR Code Asset Tracking Verification

In this step, verify real-time push notifications over WebSocket and smartphone QR code equipment scanning.

- **WebSocket:** SockJS + STOMP pushes notifications to `/user/queue/notifications`, updating unread badge counts and browser tab title.
- **QR Code:** Scanning equipment QR code opens `https://trackermaint.dpdns.org/public/equipment/{id}` without requiring login.

> [!NOTE]
> 📸 **Screenshot Placeholder:** Attach your web app screenshot showing real-time WebSocket notifications and QR code display here.
> 
> ![WebSocket and QR Code](/images/5-Workshop/5.4-S3-onprem/websocket-test.png?classes=shadow)
