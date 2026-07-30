---
title: "Tuần 4 - Kiến trúc không máy chủ: Lambda & API Gateway"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
url: "/vi/1-worklog/1.4-week4/"
---

### Chủ đề tuần

Kiến trúc không máy chủ + Cổng giao tiếp API ứng dụng

### Mục tiêu tuần

* Chuyển đổi tư duy sang kiến trúc không máy chủ để giảm thiểu công tác vận hành.
* Tích hợp chức năng xử lý mã với cổng giao tiếp API bên ngoài.

### Lịch làm việc

| Ngày | Thứ | Mô tả công việc | Nguồn tài liệu |
| :--- | :--- | :--- | :--- |
| 22/06/2026 | Thứ 2 | - Phân tích kiến trúc Serverless (Không máy chủ)<br>- Tìm hiểu vòng đời thực thi mã của AWS Lambda<br>- Khảo sát các cơ chế kích hoạt (Trigger) | https://docs.aws.amazon.com/lambda/latest/dg/ |
| 23/06/2026 | Thứ 3 | - Thực hành viết hàm Lambda:<br>&emsp;+ Tạo hàm Lambda đơn giản bằng Python/Node.js<br>&emsp;+ Thiết lập các biến môi trường<br>&emsp;+ Theo dõi kết quả xuất ra trên CloudWatch Logs | https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html |
| 24/06/2026 | Thứ 4 | - Nghiên cứu cổng giao tiếp Amazon API Gateway:<br>&emsp;+ Phân biệt RESTful API và HTTP API<br>&emsp;+ Khái niệm về Tài nguyên (Resource) và Phương thức (Method) | https://docs.aws.amazon.com/apigateway/latest/developerguide/ |
| 25/06/2026 | Thứ 5 | - Tích hợp API Gateway và Lambda:<br>&emsp;+ Định tuyến yêu cầu POST/GET đến Lambda<br>&emsp;+ Triển khai (Deploy) API lên môi trường Stage<br>&emsp;+ Kiểm thử luồng gọi bằng Postman | https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started.html |
| 26/06/2026 | Thứ 6 | - Tổng kết mô hình không máy chủ<br>- Xóa các điểm cuối API thử nghiệm tránh phát sinh phí<br>- Ghi nhận nhật ký công việc tuần 4 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả mong đợi

* Hàm Lambda thực thi mã thành công mà không cần cung cấp máy chủ vật lý hay ảo.
* Cổng API xử lý các yêu cầu HTTP và trả về phản hồi hợp lệ từ kiến trúc không máy chủ.

### Tài liệu tham khảo Tuần 4

* Hướng dẫn AWS Lambda: https://docs.aws.amazon.com/lambda/latest/dg/
* Hướng dẫn API Gateway: https://docs.aws.amazon.com/apigateway/latest/developerguide/
