---
title: "Tuần 6 - Giám sát hệ thống CloudWatch & Hạ tầng dưới dạng mã CloudFormation"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
url: "/vi/1-worklog/1.6-week6/"
---

### Chủ đề tuần

Giám sát số liệu hệ thống + Cung cấp hạ tầng dưới dạng mã

### Mục tiêu tuần

* Thu thập nhật ký và số liệu giám sát tình trạng sức khỏe của tài nguyên.
* Chuyển đổi công tác cấu hình thủ công sang kịch bản hạ tầng dưới dạng mã.

### Lịch làm việc

| Ngày | Thứ | Mô tả công việc | Nguồn tài liệu |
| :--- | :--- | :--- | :--- |
| 06/07/2026 | Thứ 2 | - Học phần giám sát hệ thống Amazon CloudWatch<br>- Phân biệt Số liệu (Metrics), Báo động (Alarms) và Nhật ký (Logs)<br>- Tìm hiểu dịch vụ thông báo SNS | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ |
| 07/07/2026 | Thứ 3 | - Thực hành giám sát tài nguyên:<br>&emsp;+ Xem chỉ số CPU của máy chủ EC2<br>&emsp;+ Đặt ngưỡng báo động gửi email khi CPU vượt mức 80%<br>&emsp;+ Chạy kịch bản tải ảo (Stress Test) để kích hoạt báo động | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html |
| 08/07/2026 | Thứ 4 | - Khái niệm Hạ tầng dưới dạng mã (Infrastructure as Code - IaC)<br>- Tìm hiểu cấu trúc mẫu (Template) của CloudFormation:<br>&emsp;+ Parameters<br>&emsp;+ Resources<br>&emsp;+ Outputs | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/ |
| 09/07/2026 | Thứ 5 | - Thực hành viết mã hạ tầng:<br>&emsp;+ Soạn kịch bản định dạng YAML để tạo VPC và Subnet<br>&emsp;+ Tải tệp lên CloudFormation Console để triển khai Ngăn xếp (Stack)<br>&emsp;+ Xác minh kết quả trên VPC Dashboard | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html |
| 10/07/2026 | Thứ 6 | - Dọn dẹp (Delete Stack) để thu hồi toàn bộ tài nguyên tự động<br>- Ghi chép nhật ký thực hành tuần 6 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả mong đợi

* Hệ thống giám sát chủ động báo động khi có các dấu hiệu bất thường về số liệu.
* Quy trình tạo lập tài nguyên được chuẩn hóa thành các tệp cấu hình có thể tái sử dụng.

### Tài liệu tham khảo Tuần 6

* Hướng dẫn Amazon CloudWatch: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/
* Hướng dẫn AWS CloudFormation: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/
