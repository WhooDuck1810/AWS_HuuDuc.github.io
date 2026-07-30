---
title: "Kiểm thử Thông báo WebSocket Real-time & Mã QR Code"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

# 5.5.4 Kiểm thử Thông báo WebSocket Real-time & Mã QR Code

Trong bước này, kiểm thử tính năng đẩy thông báo thời gian thực qua WebSocket và quét mã QR xem thông tin công khai.

- **WebSocket:** SockJS + STOMP đẩy thông báo về kênh `/user/queue/notifications`, tự động cập nhật badge đỏ và tiêu đề thẻ trình duyệt.
- **Mã QR Code:** Smartphone quét mã QR trên máy sẽ mở liên kết công khai `https://trackermaint.dpdns.org/public/equipment/{id}` xem lịch sử bảo trì không cần đăng nhập.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình ứng dụng Web hiển thị Toast thông báo WebSocket và mã QR Code đính kèm vào bên dưới.
> 
> ![WebSocket và QR Code](/images/5-Workshop/5.4-S3-onprem/websocket-test.png?classes=shadow)
