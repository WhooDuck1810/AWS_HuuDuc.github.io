---
title: "Tuần 3 - Giải pháp lưu trữ S3 & Dịch vụ cơ sở dữ liệu được quản lý RDS"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
url: "/vi/1-worklog/1.3-week3/"
---

### Chủ đề tuần

Giải pháp lưu trữ đối tượng + Dịch vụ cơ sở dữ liệu quan hệ được quản lý

### Mục tiêu tuần

* Ứng dụng giải pháp lưu trữ đối tượng để quản lý dữ liệu phi cấu trúc.
* Triển khai dịch vụ cơ sở dữ liệu được quản lý toàn diện.

### Lịch làm việc

| Ngày | Thứ | Mô tả công việc | Nguồn tài liệu |
| :--- | :--- | :--- | :--- |
| 15/06/2026 | Thứ 2 | - Khảo sát các giải pháp lưu trữ trên AWS<br>- Phân tích cấu trúc Bucket và Object trên S3<br>- Đánh giá các lớp lưu trữ (Standard, Glacier...) | https://docs.aws.amazon.com/AmazonS3/latest/userguide/ |
| 16/06/2026 | Thứ 3 | - Thực hành với dịch vụ S3:<br>&emsp;+ Bật tính năng quản lý phiên bản (Versioning)<br>&emsp;+ Cấu hình lưu trữ tĩnh cho trang HTML<br>&emsp;+ Viết Bucket Policy cấp quyền đọc công khai | https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html |
| 17/06/2026 | Thứ 4 | - Bắt đầu học phần cơ sở dữ liệu RDS:<br>&emsp;+ So sánh cơ sở dữ liệu quan hệ và phi quan hệ<br>&emsp;+ Tìm hiểu kiến trúc Đa vùng sẵn sàng (Multi-AZ)<br>&emsp;+ Chức năng Bản sao chỉ đọc (Read Replica) | https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/ |
| 18/06/2026 | Thứ 5 | - Cung cấp cơ sở dữ liệu RDS:<br>&emsp;+ Khởi tạo Engine MySQL trên RDS<br>&emsp;+ Thiết lập tài khoản admin cho database<br>&emsp;+ Chỉnh sửa Security Group để EC2 có thể truy cập | https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html |
| 19/06/2026 | Thứ 6 | - Kiểm tra kết nối cơ sở dữ liệu bằng Command Line từ EC2<br>- Dọn dẹp các bản ghi test<br>- Hoàn thiện báo cáo công việc tuần 3 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả mong đợi

* Giải pháp lưu trữ S3 được cấu hình an toàn với các vòng đời lưu trữ tự động.
* Cơ sở dữ liệu RDS sẵn sàng tiếp nhận các truy vấn kết nối từ máy chủ điện toán.

### Tài liệu tham khảo Tuần 3

* Tài liệu Amazon S3: https://docs.aws.amazon.com/AmazonS3/latest/userguide/
* Tài liệu Amazon RDS: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/
