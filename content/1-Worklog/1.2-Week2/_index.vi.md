---
title: "Tuần 2 - Nền tảng mạng VPC & Dịch vụ điện toán EC2"
date: 2026-06-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
url: "/vi/1-worklog/1.2-week2/"
---

### Chủ đề tuần

Kiến trúc Nền tảng mạng + Dịch vụ điện toán đám mây

### Mục tiêu tuần

* Nắm vững cách thiết kế một môi trường mạng ảo biệt lập an toàn.
* Khởi tạo và vận hành dịch vụ điện toán linh hoạt thông qua máy chủ ảo.

### Lịch làm việc

| Ngày | Thứ | Mô tả công việc | Nguồn tài liệu |
| :--- | :--- | :--- | :--- |
| 08/06/2026 | Thứ 2 | - Bắt đầu học phần Nền tảng mạng VPC<br>- Tìm hiểu cấu trúc khối IP (CIDR Block)<br>- Phân biệt mạng con công cộng (Public) và riêng tư (Private) | https://docs.aws.amazon.com/vpc/latest/userguide/ |
| 09/06/2026 | Thứ 3 | - Thực hành thiết lập VPC:<br>&emsp;+ Tạo 1 VPC mới với dải IP 10.0.0.0/16<br>&emsp;+ Cấu hình Cổng kết nối Internet (IGW)<br>&emsp;+ Cập nhật bảng định tuyến (Route Table) | https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html |
| 10/06/2026 | Thứ 4 | - Tìm hiểu về dịch vụ điện toán linh hoạt EC2:<br>&emsp;+ Phân loại Instance (T3, M5, C5...)<br>&emsp;+ Tìm hiểu Amazon Machine Image (AMI)<br>&emsp;+ Ổ cứng đàn hồi (EBS) | https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ |
| 11/06/2026 | Thứ 5 | - Khởi chạy một phiên bản EC2:<br>&emsp;+ Tạo cặp khóa SSH (Key Pair)<br>&emsp;+ Cấu hình nhóm bảo mật mở cổng 22<br>&emsp;+ Đăng nhập từ xa vào máy chủ Linux | https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html |
| 12/06/2026 | Thứ 6 | - Đánh giá tổng quan về dịch vụ mạng và điện toán<br>- Dừng (Stop) máy chủ EC2 để tiết kiệm chi phí<br>- Ghi nhận tiến độ tuần 2 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả mong đợi

* Môi trường mạng VPC được định tuyến và phân chia mạng con chính xác.
* Máy chủ điện toán EC2 hoạt động ổn định và có thể truy cập an toàn.

### Tài liệu tham khảo Tuần 2

* Tài liệu VPC gốc: https://docs.aws.amazon.com/vpc/latest/userguide/
* Tài liệu EC2 gốc: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/
