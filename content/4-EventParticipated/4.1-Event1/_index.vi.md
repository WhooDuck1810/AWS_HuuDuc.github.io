---
title: "Event 1: AWS Agentic AI Build Week (AABW)"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Báo cáo sự kiện: “AWS Agentic AI Build Week (AABW)”

### Thông tin tổng quan
- **Tên sự kiện:** AWS Agentic AI Build Week (AABW)
- **Thời gian:** 27/07/2026
- **Địa điểm:** Tầng 26, Tòa nhà Bitexco Financial Tower, Số 02 Hải Triều, Phường Sài Gòn, TP. Hồ Chí Minh
- **Vai trò:** Người tham dự

---

### 1. Mục tiêu sự kiện

Sự kiện **AWS Agentic AI Build Week (AABW)** là một thử thách Hackathon kéo dài 24 giờ tập trung vào việc ứng dụng trí tuệ nhân tạo thế hệ mới (Agentic AI) để giải quyết các bài toán thực tế. Mục tiêu chính của sự kiện bao gồm:

- **Phát triển sản phẩm siêu tốc:** Xây dựng một sản phẩm khả thi tối thiểu (MVP) hoàn chỉnh dưới áp lực thời gian gay gắt.
- **Trải nghiệm thực tế Cloud & AI:** Trực tiếp thao tác và kết hợp các dịch vụ điện toán đám mây AWS với các mô hình AI chuyên sâu.
- **Tiếp cận hệ thống thực tế:** Bước ra khỏi lý thuyết học đường để hiểu rõ cách một hệ thống quy mô doanh nghiệp vận hành.
- **Kỹ năng làm việc nhóm hiệu quả:** Rèn luyện khả năng làm việc nhóm từ khâu lên ý tưởng, phân chia công việc, lập trình cho đến thuyết trình demo sản phẩm.

---

### 2. Phân tích các dự án nổi bật tại Hackathon

Trong suốt cuộc thi, em đã có cơ hội quan sát và phân tích 3 dự án tiêu biểu ứng dụng Agentic AI trong nhiều lĩnh vực:

#### 2.1. Đội 3KA – Dự án S.H.E.P.H.E.R.D (Quản lý đám đông thông minh)
- **Bài toán:** Quản lý đám đông thủ công tại các sự kiện lớn thường bị chậm trễ, khó mở rộng và dễ bỏ sót các rủi ro mất an toàn.
- **Giải pháp:** Xây dựng hệ thống camera AI thông minh theo dõi luồng người, đo lường mật độ, dự báo điểm nghẽn và đưa ra cảnh báo chủ động.
- **Công nghệ sử dụng:** YOLO, ByteTrack, Amazon SageMaker, Amazon Bedrock Agent, Core + Strands Agent Framework, và React Dashboard.
- **Kiến trúc Agentic AI:** Gồm 2 lớp chính:
  1. *Lớp Giám sát Tự động (Autonomous Monitor):* Liên tục theo dõi và phân tích luồng dữ liệu hình ảnh theo thời gian thực.
  2. *Lớp Trợ lý Vận hành (Operator Copilot):* Cho phép nhân viên sử dụng câu hỏi ngôn ngữ tự nhiên để truy xuất dữ liệu vận hành tức thì.

#### 2.2. Đội Signal Scout – Hệ thống Phân tích Chiến lược Doanh nghiệp
- **Bài toán:** Dữ liệu chiến lược và chỉ số vận hành của doanh nghiệp thường bị phân mảnh, khó kết nối thành bức tranh tổng thể.
- **Giải pháp:** Tự động thu thập, phân tích chỉ số tài chính/vận hành và dùng AI để phát hiện sớm các tín hiệu tái cấu trúc hoặc chuyển hướng chiến lược.
- **Điểm sáng kiến trúc:** Tập trung vào triển khai phi tập trung, khả năng quan sát hệ thống (Observability), và quy trình CI/CD chuẩn chỉnh. Đội thi cũng trình bày bảng phân tích chi phí vận hành chi tiết trên AWS (từ ~$81/tháng ở mức tối thiểu đến ~$359/tháng khi tải cao) cùng các phương án tối ưu chi phí.

#### 2.3. Đội Plan V – Ứng dụng Trợ lý Kỹ sư Giải pháp (SA Professional App)
- **Bài toán:** Các Kỹ sư Giải pháp (Solution Architects - SA) thường mất nhiều thời gian thủ công để đọc tài liệu yêu cầu (BRD/PRD), vẽ sơ đồ kiến trúc và tính toán chi phí hệ thống.
- **Giải pháp:** Trợ lý AI tự động đọc tài liệu, phác thảo kiến trúc hệ thống, tự động tạo sơ đồ AWS/Draw.io có thể chỉnh sửa, và ước tính chi phí cho khu vực `ap-southeast-1`.
- **Tác động:** Giúp các SA tương tác qua giao diện chat để tinh chỉnh và tự động tạo ra các kịch bản Infrastructure as Code (IaC) chỉ trong vài phút.

---

### 3. Bài học Kỹ thuật Thu hoạch được

- **Tích hợp hệ thống phức tạp:** Đội 3KA mang lại bài học quý giá về cách kết hợp giữa xử lý thị giác máy tính theo thời gian thực (YOLO), mô hình AI trên Cloud (SageMaker, Bedrock) và giao diện điều khiển (React) mà vẫn đảm bảo độ trễ thấp.
- **Tối ưu chi phí Cloud (FinOps):** Bảng phân tích chi tiết của đội Signal Scout (dựa trên lượng token Bedrock, vCPU, WAF) nhắc nhở em rằng một hệ thống tốt không chỉ chạy đúng mà còn phải tối ưu về mặt chi phí vận hành.
- **Tự động hóa quy trình phát triển:** Giải pháp của đội Plan V chứng minh sức mạnh của AI trong việc tự động tạo ra các tài liệu kỹ thuật (sơ đồ kiến trúc, mã IaC), giúp tăng tốc đáng kể quy trình phát triển phần mềm.

---

### 4. Bài học về Quản lý & Làm việc Nhóm

Trải nghiệm quan sát một cuộc thi 24h mang lại những bài học sâu sắc về tâm lý và kỹ năng nhóm:

- **Ứng phó với sự cố:** Các đội thi đều trải qua những giây phút căng thẳng khi gặp lỗi Git conflict, chạm ngưỡng giới hạn API, hay sự cố lộ file cấu hình. Việc giữ bình tĩnh và đưa tâm trí vào trạng thái tập trung cao độ (flow state) đã giúp các đội vượt qua giai đoạn hoảng loạn ban đầu.
- **4 Trụ cột chuẩn bị:** Một quá trình Hackathon trơn tru đòi hỏi: Mục tiêu rõ ràng, Bộ template/bộ khung dựng sẵn, Phân công nhiệm vụ cụ thể, và Luyện tập trước bản demo.
- **Tư duy hoàn thành:** Bài học đắt giá nhất là *"Có mặt và hoàn thành đã là chiến thắng một nửa"*. Một tính năng nhỏ chạy mượt mà có giá trị hơn nhiều so với một ý tưởng hoành tráng nhưng không demo được.

---

### 5. Ứng dụng vào Thực tế Công việc

- **Nâng cao Bảo mật Dự án Tracker Maintenance:** Sự cố lộ file `.env` của đội thi là một lời nhắc nhở đắt giá, giúp em chú trọng hơn vào cấu hình `.gitignore` và quy trình thu hồi API key ngay lập tức — điều em đã áp dụng trực tiếp khi xây dựng luồng xác thực JWT cho dự án Tracker Maintenance.
- **Tư duy Kiến trúc Trước Lập trình:** Em áp dụng thói quen vẽ sơ đồ kiến trúc và tính toán chi phí Cloud (tương tự ứng dụng SA của Plan V) trước khi bắt tay vào viết code cho bất kỳ dự án AWS nào.

