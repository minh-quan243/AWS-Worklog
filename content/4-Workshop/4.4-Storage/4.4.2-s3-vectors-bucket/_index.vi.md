---
title : "S3 Vectors Bucket"
date: "2000-01-01"
weight: 2
chapter : false
pre : " <b> 4.4.2. </b> "
---

Bucket này lưu trữ chỉ mục vector ngữ nghĩa (semantic vector index) cho từng bản ghi âm, bao gồm:
- Các vector nhúng của từng đoạn (chunk embeddings) (để truy xuất thông tin thực tế chính xác)
- Các bản tóm tắt cấp độ phân đoạn (để cho các truy vấn ở cấp độ chủ đề)
- Một bản tóm tắt tổng thể (để cho các truy vấn tóm tắt toàn bộ cuộc họp)

Backend EC2 đọc và ghi trực tiếp vào bucket này thông qua VPC Gateway Endpoint đã được tạo trong phần thiết lập mạng.

---

#### Bước 1 — Nhấp Create Bucket

Điều hướng đến **S3** trong AWS Console. Nhấp vào **Create bucket**.

![Nhấp Create Vectors Bucket](/images/4-Workshop/S3_Vector/B1_Click_Create_Vector_Bucket.png)

---

#### Bước 2 — Nhập Tên Bucket và Tạo

- **Bucket name**: Chọn một tên duy nhất, ví dụ: `voice-summarizer-vectors-<your-account-id>`
- **AWS Region**: `ap-southeast-1`
- **Block all public access**: ✅ Đã bật (Enabled)
- **Default encryption**: SSE-S3

Lưu ý tên bucket — bạn sẽ cần cấu hình nó dưới dạng biến môi trường trên EC2 instance.

Nhấp vào **Create bucket**.

![Nhập Tên và Tạo](/images/4-Workshop/S3_Vector/B2_Create_Bucket.png)

---

{{% notice tip %}}
✅ Bạn hiện đã cấu hình xong hai bucket S3: <br>- **Bucket Âm thanh & Bản phiên âm** (`one4allthing`) với các thư mục `raw_audio/` và `transcripts/` <br>- **Bucket Vectors** để lưu trữ chỉ mục ngữ nghĩa
{{% /notice %}}