---
title : "Tổng quan"
date: "2026-04-04" 
weight: 1
chapter : false
pre : " <b> 4.1. </b> "
---

#### Tổng quan Dự án

**Voice Summarizer** giải quyết một vấn đề phổ biến trong các tổ chức hiện đại: những thông tin có giá trị bị kẹt trong các bản ghi âm thực tế là không thể tiếp cận được sau khi sự việc diễn ra. Việc phát lại hàng giờ các cuộc họp chỉ để tìm một quyết định hoặc một mục hành động nào đó là điều không thực tế.

Nền tảng này tự động hóa toàn bộ quy trình — từ tải lên âm thanh gốc đến một giao diện kiến thức tương tác được hỗ trợ bởi AI — vì vậy bất kỳ từ nào được nói ra trong bất kỳ phiên ghi âm nào đều có thể được truy xuất ngay lập tức thông qua ngôn ngữ tự nhiên.

#### Các Tính năng Cốt lõi của Người dùng

**1. Quy trình Tự động chuyển Âm thanh thành Kiến thức**

Người dùng tải lên tệp âm thanh (WAV, MP3, M4A) thông qua URL S3 được ký trước (presigned URL) trực tiếp lên lưu trữ đám mây. Một luồng xử lý worker bất đồng bộ sẽ tiếp nhận tệp tải lên, gửi nó đến AWS Transcribe, đợi hoàn tất, chia nhỏ và nhúng (embed) bản phiên âm, đồng thời lưu trữ các vector trong một chỉ mục ngữ nghĩa (semantic index) — tất cả diễn ra mà không cần sự can thiệp của người dùng.

**2. Trợ lý AI Đàm thoại**

Mỗi bản ghi âm sẽ có giao diện trò chuyện riêng. Người dùng đặt câu hỏi bằng ngôn ngữ tự nhiên; hệ thống sẽ phân loại ý định, chọn chiến lược truy xuất phù hợp, lấy ra các đoạn phiên âm có liên quan nhất và tạo câu trả lời có cơ sở từ LLM. Bộ nhớ cuộc hội thoại được lưu giữ xuyên suốt các phiên.

**3. Thư viện & Bảng điều khiển Bản ghi âm**

Một thư viện có thể tìm kiếm liệt kê tất cả các bản ghi âm đã được xử lý cùng với các bản tóm tắt do AI tạo ra, trạng thái xử lý, thời lượng và ngày tạo.

#### Kiến trúc Hệ thống

Nền tảng được triển khai tại **ap-southeast-1 (Singapore)** và sử dụng kiến trúc lai kết hợp giữa các trigger không máy chủ (serverless) với một backend được lưu trữ trên EC2:

![Tổng quan Kiến trúc](/images/2-Proposal/AWS_Diagram_Flow.jpg)

**Luồng lưu lượng (được đánh số như trong sơ đồ):**

- **(0)** Truy vấn DNS của người dùng được phân giải qua **Amazon Route 53**, dịch vụ này sẽ ánh xạ tên miền tùy chỉnh đến các endpoint của nền tảng
- **(1)** Route 53 định tuyến trình duyệt đến frontend React được lưu trữ trên **AWS Amplify**
- **(2)** Người dùng xác thực thông qua **Amazon Cognito**; JWT tokens được cấp và lưu trữ trong phiên trình duyệt
- **(3)** Trigger sau khi xác nhận (post-confirmation) của Cognito sẽ kích hoạt **Lambda** (`user_creation_db`) → tạo bản ghi người dùng trong **DynamoDB** (bảng `User`)
- **(4)** Các lời gọi API từ frontend sẽ đi đến **Application Load Balancer (ALB)** trong subnet công khai (public subnet) thông qua bản ghi bí danh (alias record) API của Route 53
- **(5)** ALB định tuyến lưu lượng qua các **VPC Gateway Endpoints** vào subnet riêng (private subnet)
- **(6)** Gateway Endpoints chuyển tiếp yêu cầu đến **EC2 instance** (FastAPI + Celery + Redis)
- **(7)** EC2 đọc/ghi vào **DynamoDB** (`voice_status_table`, `memory_AI`) và **S3** (Bản phiên âm, Vectors, Bản tóm tắt) qua Gateway Endpoints
- **(8)** Các tệp âm thanh được tải trực tiếp lên **S3 (bucket Âm thanh)** thông qua URL được ký trước từ frontend
- **(9)** Thông báo sự kiện S3 kích hoạt **Lambda chuyển đổi âm thanh thành văn bản** (`audio2text`)
- **(10)** Lambda bắt đầu một công việc **AWS Transcribe**
- **(11)** Transcribe ghi bản phiên âm đã hoàn tất vào **S3 (bucket Bản phiên âm)**
- **(12)** EC2 Celery worker đọc bản phiên âm từ S3 để bắt đầu quá trình vector hóa
- **(13)** EC2 ghi và đọc chỉ mục vector từ **S3 (bucket Vectors)**
- **(14)** Các lời gọi API LLM từ EC2 đi ra ngoài thông qua **NAT Gateway → Internet Gateway → LLM API**

#### Các Dịch vụ Bạn sẽ Cấu hình

| Dịch vụ | Mục đích |
|---|---|
| Amazon Route 53 | DNS cho tên miền tùy chỉnh; các bản ghi bí danh (alias) định tuyến đến Amplify và ALB |
| Amazon VPC | Cách ly mạng với các subnet công khai (public) và riêng (private) |
| Internet Gateway | Truy cập internet đến/đi cho subnet công khai |
| NAT Gateway | Truy cập internet chỉ đi ra (outbound-only) cho subnet riêng (các lời gọi API LLM) |
| VPC Gateway Endpoints | Kết nối riêng đến S3 và DynamoDB từ subnet riêng |
| Security Group | Quy tắc tường lửa cho EC2 |
| AWS S3 (×1, bốn prefix) | Một bucket duy nhất `one4allthing` với các tiền tố `raw_audio/`, `transcripts/`, `vectors/`, và `summarize/` |
| Amazon DynamoDB (×1) | Bảng duy nhất lưu trữ trạng thái bản ghi âm, bộ nhớ hội thoại AI và bản ghi người dùng |
| AWS Lambda (×1) | Trigger chuyển đổi âm thanh thành văn bản |
| AWS EC2 | Máy tính backend: máy chủ API FastAPI, Celery worker, Redis |
| Application Load Balancer | Định tuyến lưu lượng API từ frontend tới EC2 instance trong subnet riêng |
| Amazon Cognito | Nhóm người dùng (User pool) với xác thực dựa trên JWT |
| AWS Amplify | Lưu trữ và triển khai frontend React từ GitHub |

#### Thời gian Dự kiến

| Hạng mục | Thời lượng Dự kiến |
|---|---|
| VPC & Networking | ~25 phút |
| Lưu trữ (S3 + DynamoDB) | ~15 phút |
| Các Hàm Lambda | ~15 phút |
| Thiết lập EC2 | ~10 phút |
| Thiết lập ALB | ~10 phút |
| Thiết lập Cognito | ~10 phút |
| Triển khai Amplify | ~10 phút |
| **Tổng cộng** | **~95 phút** |