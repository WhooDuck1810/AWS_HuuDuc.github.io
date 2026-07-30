---
title: "Kết quả kiểm thử & Thực nghiệm"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 4. Kết quả kiểm thử & Thực nghiệm

## 4.1 Unit Test — LoginAttemptService

**File kiểm thử:** `tracker_maintenance_service/src/test/java/.../service/LoginAttemptServiceTest.java`

Toàn bộ 5 kịch bản Unit Test đều vượt qua với tỷ lệ thành công 100% khi chạy lệnh `.\mvnw.cmd test`:

| Kịch bản Test | Mô tả | Kết quả |
|---|---|---|
| `testUsernameNotBlockedInitially()` | Tên người dùng mới chưa có lần đăng nhập sai sẽ không bị khóa | ✅ ĐẠT |
| `testUsernameBlockedAfterMaxAttempts()` | Sau 5 lần đăng nhập sai liên tiếp, tên người dùng sẽ bị khóa | ✅ ĐẠT |
| `testIpBlockedAfterMaxAttempts()` | Sau 20 lần đăng nhập sai từ cùng 1 IP, địa chỉ IP sẽ bị khóa | ✅ ĐẠT |
| `testSuccessfulLoginResetsAttempts()` | Đăng nhập thành công sẽ reset đếm số lần sai về 0 | ✅ ĐẠT |
| `testUnblockAfterLockoutPeriod()` | Sau khi hết 15 phút khóa tạm thời, tài khoản được tự động mở khóa | ✅ ĐẠT |

**Kết xuất kết quả (Output):**
```
[INFO] Tests run: 5, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

## 4.2 Bảo vệ Brute-Force — Kiểm thử thủ công

**Kịch bản kiểm thử:** Thử đăng nhập liên tục với mật khẩu sai 6 lần.

| Lần thử | Username | Password | Mã phản hồi HTTP |
|---|---|---|---|
| 1st | admin | wrongpass | `401 Unauthorized` — Sai thông tin xác thực |
| 2nd | admin | wrongpass | `401 Unauthorized` — Sai thông tin xác thực |
| 3rd | admin | wrongpass | `401 Unauthorized` — Sai thông tin xác thực |
| 4th | admin | wrongpass | `401 Unauthorized` — Sai thông tin xác thực |
| 5th | admin | wrongpass | `401 Unauthorized` — Sai thông tin xác thực |
| 6th | admin | wrongpass | `429 Too Many Requests` — **Tài khoản bị khóa** |
| 7th (Đúng) | admin | Admin123! | `429 Too Many Requests` — **Vẫn bị khóa** |

**Kết luận:** Tài khoản bị khóa chính xác sau 5 lần nhập sai liên tiếp, ngay cả khi nhập đúng mật khẩu ở lần thứ 7 cũng không thể vượt qua trong khoảng thời gian 15 phút. ✅

## 4.3 Luồng CI/CD Pipeline — Kiểm thử triển khai

**Thực nghiệm:** Push một thay đổi nhỏ lên nhánh `main` và theo dõi luồng triển khai tự động.

**Tiến trình chạy trên GitHub Actions:**
```
00:00 - Push commit lên main
00:02 - GitHub Actions Workflow được kích hoạt
00:15 - Xắc thực quyền AWS Credentials thành công
00:30 - Bắt đầu Build Docker Backend
02:30 - File JAR Backend được đóng gói (Maven compile + package)
03:00 - Push Docker image Backend lên ECR ap-southeast-2
03:05 - Bắt đầu Build Docker Frontend
04:00 - Build xong ứng dụng React Frontend
04:20 - Push Docker image Frontend lên ECR ap-southeast-2
04:25 - Kết nối SSH thành công vào EC2 (3.106.194.112)
04:35 - Đăng nhập ECR trên EC2 thành công
04:40 - Chạy docker-compose pull tải image mới về
04:55 - Chạy docker-compose up -d khởi động lại container
05:00 - Xóa các Docker image cũ không dùng (prune)
05:05 - Workflow thành công THÀNH CÔNG ✅
```

**Tổng thời gian triển khai:** ~5 phút từ lúc `git push` đến khi hệ thống cập nhật live trên production. ✅

## 4.4 Thông báo thời gian thực — Kiểm thử WebSocket

**Kịch bản:**
1. Mở Thẻ trình duyệt 1 (Đăng nhập vai trò MANAGER) và Thẻ 2 (Đăng nhập vai trò TECHNICIAN).
2. Manager tạo mới một ticket bảo trì và phân công cho Technician.

**Kết quả thực tế:**
- Thẻ 2 (Technician) hiển thị thông báo dạng Toast ngay lập tức mà không cần tải lại trang.
- Tiêu đề Thẻ 2 tự động cập nhật từ `My Tickets | Tracker Maintenance` thành `(1) My Tickets | Tracker Maintenance`.
- Biểu tượng quả chuông hiển thị badge màu đỏ với số `1`. ✅

## 4.5 Tải ảnh lên S3 — Kiểm thử

**Thực nghiệm:** Tải lên một ảnh thiết bị dạng JPEG từ trang Chi tiết Thiết bị.

- Sau 1.2 giây, ảnh được tải thành công lên S3 và giao diện hiển thị ảnh trực tiếp từ đường dẫn công khai:
`https://tracker-maintenance-images-123.s3.ap-southeast-2.amazonaws.com/equipment/eq-001/photo-1234567890.jpg` ✅

## 4.6 Quét mã QR — Kiểm thử

**Thực nghiệm:** Dùng smartphone quét mã QR dán trên thiết bị.

**Kết quả:** Trình duyệt di động mở liên kết `https://trackermaint.dpdns.org/public/equipment/1` và hiển thị đầy đủ thông tin thiết bị, trạng thái, model, số sê-ri, vị trí và lịch sử bảo trì mà không yêu cầu đăng nhập. ✅

## 4.7 Log tập trung CloudWatch — Kiểm thử

Log Group `/tracker-maintenance/backend` xuất hiện tự động trên CloudWatch Console và hiển thị đầy đủ thông tin SQL, thông báo hệ thống và lịch sử đăng nhập theo thời gian thực. ✅

## 4.8 Sao chép ECR đa vùng — Kiểm thử

Sau khi push Docker image lên ECR Sydney, chỉ trong vòng 60 giây, cả 2 repository `tracker-be` và `tracker-fe` tự động xuất hiện tại ECR `ap-southeast-1` (Singapore) mà không cần thao tác thủ công. ✅