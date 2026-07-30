---
title: "Tuần 8 - Cấp phát tài nguyên dự án & Thiết lập luồng triển khai liên tục"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
url: "/vi/1-worklog/1.8-week8/"
---

### Chủ đề tuần

Cấp phát tài nguyên đám mây + Tích hợp và Triển khai liên tục

### Mục tiêu tuần

* Chuyển hóa sơ đồ kiến trúc thành hệ thống hạ tầng thực tế trên nền tảng đám mây.
* Thiết lập luồng triển khai liên tục tự động hóa việc đưa mã nguồn lên môi trường.

### Lịch làm việc

| Ngày | Thứ | Mô tả công việc | Nguồn tài liệu |
| :--- | :--- | :--- | :--- |
| 20/07/2026 | Thứ 2 | - Triển khai tài nguyên nền tảng mạng:<br>&emsp;+ Khởi tạo VPC, Internet Gateway, NAT Gateway<br>&emsp;+ Cấu hình chính xác các bảng định tuyến (Route Table)<br>&emsp;+ Thiết lập các nhóm bảo mật (Security Groups) | Theo sơ đồ kiến trúc T7 |
| 21/07/2026 | Thứ 3 | - Triển khai dịch vụ điện toán:<br>&emsp;+ Cài đặt hệ điều hành trên máy chủ EC2<br>&emsp;+ Cấu hình bộ cân bằng tải đàn hồi (ELB) nếu cần<br>&emsp;+ Đẩy mã nguồn Backend lên máy chủ | Hướng dẫn triển khai Backend |
| 22/07/2026 | Thứ 4 | - Triển khai dữ liệu và lưu trữ tĩnh:<br>&emsp;+ Khởi tạo và kết nối cơ sở dữ liệu RDS/DynamoDB<br>&emsp;+ Tải mã nguồn Frontend tĩnh lên bucket S3<br>&emsp;+ Liên kết API giữa Frontend và Backend | Hướng dẫn triển khai DB |
| 23/07/2026 | Thứ 5 | - Cấu hình đường ống tích hợp liên tục (CI/CD):<br>&emsp;+ Kết nối mã nguồn từ GitHub vào AWS CodePipeline<br>&emsp;+ Định nghĩa quy trình build tự động (CodeBuild)<br>&emsp;+ Thiết lập tiến trình deploy tự động (CodeDeploy) | https://docs.aws.amazon.com/codepipeline/latest/userguide/ |
| 24/07/2026 | Thứ 6 | - Xác minh sự hoạt động thông suốt của các điểm cuối (Endpoints)<br>- Xử lý các lỗi kết nối bước đầu<br>- Ghi nhận tiến độ tuần 8 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả mong đợi

* Toàn bộ kiến trúc hạ tầng được cung cấp hoàn tất và sẵn sàng phục vụ lưu lượng mạng.
* Đường ống triển khai hoạt động ổn định, tự động biên dịch và phân phối mã nguồn.

### Tài liệu tham khảo Tuần 8

* Hướng dẫn AWS CodePipeline: https://docs.aws.amazon.com/codepipeline/latest/userguide/
* Thực hành DevOps trên AWS: https://aws.amazon.com/devops/
