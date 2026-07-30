---
title: "Kết quả kiểm thử & Thực nghiệm"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. Kết quả kiểm thử & Thực nghiệm

Sau khi hoàn tất triển khai hạ tầng và luồng CI/CD, tiến hành các bài kiểm thử thực nghiệm để xác minh tính năng, bảo mật và khả năng vận hành thời gian thực.

---

### Bước 5.4.1: Unit Test — Xác minh dịch vụ LoginAttemptService

Chạy kịch bản kiểm thử đơn vị Backend:
```bash
.\mvnw.cmd test
```

| Kịch bản Test | Mô tả | Kết quả |
|---|---|---|
| `testUsernameNotBlockedInitially()` | User mới chưa sai 0 lần không bị khóa | ✅ ĐẠT |
| `testUsernameBlockedAfterMaxAttempts()` | Khóa tài khoản sau 5 lần sai liên tiếp | ✅ ĐẠT |
| `testIpBlockedAfterMaxAttempts()` | Khóa IP sau 20 lần sai liên tiếp | ✅ ĐẠT |
| `testSuccessfulLoginResetsAttempts()` | Đăng nhập thành công reset bộ đếm | ✅ ĐẠT |
| `testUnblockAfterLockoutPeriod()` | Tự mở khóa sau 15 phút | ✅ ĐẠT |

---

### Bước 5.4.2: Bảo vệ Brute-Force — Thử nghiệm Khóa tài khoản

Giả lập tấn công dò mật khẩu tự động vào API `/auth/token`:

| Lần thử | Mật khẩu nhập | Mã phản hồi HTTP |
|---|---|---|
| Lần 1 – 5 | `wrongpass` | `401 Unauthorized` |
| Lần 6 | `wrongpass` | `429 Too Many Requests` (Tài khoản bị khóa) |
| Lần 7 | `Admin123!` (Đúng) | `429 Too Many Requests` (Vẫn bị khóa trong 15p) |

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình phản hồi HTTP 429 Too Many Requests trên Postman hoặc trình duyệt và dán vào bên dưới.
> 
> ![Kiểm thử Chống Brute Force](/images/5-Workshop/5.4-S3-onprem/brute-force-test.png?classes=shadow)

---

### Bước 5.4.3: Kiểm thử Thông báo Real-Time WebSocket

1. Mở Dashboard Quản lý ở Thẻ 1 và Giao diện Kỹ thuật viên ở Thẻ 2.
2. Manager tạo và phân công ticket bảo trì cho Technician.

**Kết quả:**
- Thẻ 2 nhận thông báo Toast thời gian thực không cần F5 tải lại trang.
- Tiêu đề tab đổi dạng `(1) My Tickets | Tracker Maintenance`.
- Quả chuông hiển thị badge số đỏ `1`. ✅

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình ứng dụng Web hiển thị Toast thông báo WebSocket và tiêu đề tab đính kèm vào bên dưới.
> 
> ![Kiểm thử WebSocket Realtime](/images/5-Workshop/5.4-S3-onprem/websocket-test.png?classes=shadow)

---

### Bước 5.4.4: Kiểm thử Upload Ảnh S3 & Quét mã QR Code

1. Tải ảnh thiết bị từ trang Chi tiết Thiết bị.
2. Dùng camera smartphone quét mã QR dán trên máy.

**Kết quả:**
- Ảnh tải lên trực tiếp S3 và hiển thị qua URL `https://tracker-maintenance-images-123.s3.ap-southeast-2.amazonaws.com/equipment/eq-001/photo.jpg`.
- Smartphone mở trang `https://trackermaint.dpdns.org/public/equipment/1` hiển thị đầy đủ thông tin mà không cần đăng nhập. ✅

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình hiển thị mã QR Code và ảnh tải lên S3 trên giao diện đính kèm vào bên dưới.
> 
> ![Kiểm thử S3 và mã QR](/images/5-Workshop/5.4-S3-onprem/qrcode-s3-test.png?classes=shadow)

---

### Bước 5.4.5: Kiểm thử Giám sát Log CloudWatch

Xác minh log ứng dụng đẩy về CloudWatch Logs thời gian thực:
- Backend Log Group: `/tracker-maintenance/backend`
- Frontend Log Group: `/tracker-maintenance/frontend`

> [!NOTE]
> 📸 **Vị trí chèn ảnh màn hình:** Chụp ảnh màn hình CloudWatch Logs Console xem các dòng stream log thực tế đính kèm vào bên dưới.
> 
> ![Kiểm thử CloudWatch Logs](/images/5-Workshop/5.4-S3-onprem/cloudwatch-logs-test.png?classes=shadow)