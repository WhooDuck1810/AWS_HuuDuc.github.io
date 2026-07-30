---
title: "Triển khai Cơ chế Chống tấn công Brute-Force"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3 Triển khai Cơ chế Chống tấn công Brute-Force (HTTP 429)

Trong bước này, kiểm thử cơ chế khóa tài khoản lưu trên RAM bảo vệ endpoint `/auth/token`.

- **Nguyên lý:** `LoginAttemptService` sử dụng `ConcurrentHashMap` trên RAM để đếm số lần sai theo Username và IP.
- **Quy tắc:** Tự động khóa 15 phút và trả về mã lỗi `HTTP 429 Too Many Requests` khi đăng nhập sai quá 5 lần.

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình phản hồi HTTP 429 trên Postman hoặc trình duyệt đính kèm vào bên dưới.
> 
> ![Chống tấn công Brute Force](/images/5-Workshop/5.4-S3-onprem/brute-force-test.png?classes=shadow)
