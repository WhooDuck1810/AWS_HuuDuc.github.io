---
title: "Tuần 5 - Kiến thức cơ bản về DynamoDB & Hạ tầng toàn cầu CloudFront"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
url: "/vi/1-worklog/1.5-week5/"
---

### Chủ đề tuần

Cơ sở dữ liệu phi quan hệ + Mạng phân phối nội dung hạ tầng toàn cầu

### Mục tiêu tuần

* Hiểu và thao tác với dịch vụ cơ sở dữ liệu phi quan hệ hiệu suất cao.
* Tận dụng hạ tầng toàn cầu để giảm độ trễ truy cập nội dung web.

### Lịch làm việc

| Ngày | Thứ | Mô tả công việc | Nguồn tài liệu |
| :--- | :--- | :--- | :--- |
| 29/06/2026 | Thứ 2 | - Nắm bắt mô hình cơ sở dữ liệu DynamoDB (NoSQL)<br>- Tìm hiểu Khóa phân vùng (Partition Key) và Khóa sắp xếp (Sort Key)<br>- Đọc tài liệu về dung lượng tính toán (RCU/WCU) | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ |
| 30/06/2026 | Thứ 3 | - Thực hành thao tác trên DynamoDB:<br>&emsp;+ Khởi tạo bảng dữ liệu người dùng<br>&emsp;+ Dùng giao diện để thêm (PutItem) và lấy (GetItem) dữ liệu<br>&emsp;+ Phân biệt truy vấn Scan và Query | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html |
| 01/07/2026 | Thứ 4 | - Tìm hiểu dịch vụ DNS Amazon Route 53<br>- Tìm hiểu kiến trúc mạng phân phối nội dung CloudFront<br>- Vai trò của các Điểm biên (Edge Locations) | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/ |
| 02/07/2026 | Thứ 5 | - Cấu hình mạng phân phối nội dung:<br>&emsp;+ Khởi tạo CloudFront Distribution mới<br>&emsp;+ Trỏ Nguồn gốc (Origin) về Bucket S3 tĩnh<br>&emsp;+ Cấu hình chính sách Cache Behavior | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/GettingStarted.html |
| 03/07/2026 | Thứ 6 | - Đánh giá hiệu suất truy cập qua link CloudFront so với link S3 gốc<br>- Hủy bỏ các cấu hình Route 53 nháp<br>- Tóm tắt tiến độ tuần 5 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả mong đợi

* Bảng dữ liệu DynamoDB được thiết kế đúng chuẩn hiệu suất cao với khóa phân vùng hợp lý.
* Nội dung tĩnh được lưu vào bộ nhớ đệm tại các điểm biên của hạ tầng toàn cầu.

### Tài liệu tham khảo Tuần 5

* Tài liệu Amazon DynamoDB: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/
* Tài liệu Amazon CloudFront: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/
