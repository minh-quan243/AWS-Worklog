---
title : "S3 Audio & Transcript Bucket"
date: "2000-01-01"
weight: 1
chapter : false
pre : " <b> 4.4.1. </b> "
---

Bucket này lưu trữ hai loại dữ liệu dưới các tiền tố (prefixes) riêng biệt:
- `raw_audio/` — các tệp âm thanh gốc được người dùng tải lên
- `transcripts/` — đầu ra bản phiên âm dạng JSON từ AWS Transcribe

Hàm Lambda sử dụng tên bucket `one4allthing` được mã hóa cứng (hardcoded) trong cấu hình của nó. Hãy sử dụng tên này (hoặc cập nhật mã Lambda để khớp với tên bạn chọn).

---

#### Bước 1 — Mở S3 Console và Tạo một Bucket

Điều hướng đến **S3** trong AWS Console. Nhấp vào **Create bucket**.

![Nhấp Create Bucket](/images/4-Workshop/S3_Bucket/B1_Click_Create_Bucket.png)

---

#### Bước 2 — Nhập Tên Bucket

- **Bucket name**: `one4allthing`
- **AWS Region**: `ap-southeast-1`

{{% notice info %}}
Tên bucket phải là duy nhất trên toàn cầu (globally unique). Nếu `one4allthing` đã được sử dụng, hãy chọn một tên duy nhất và cập nhật biến `OUTPUT_BUCKET` trong mã hàm Lambda `audio2text` cho phù hợp.
{{% /notice %}}

![Nhập Tên Bucket](/images/4-Workshop/S3_Bucket/B2_Naming.png)

---

#### Bước 3 — Chặn Tất cả Truy cập Công khai (Block All Public Access)

Trong phần **Block Public Access settings for this bucket**, đảm bảo cả bốn hộp kiểm đều được **đánh dấu** (chặn tất cả truy cập công khai). Bucket này tuyệt đối không bao giờ được phép truy cập công khai.

![Chặn Truy cập Công khai](/images/4-Workshop/S3_Bucket/B3_Block_All_Public_Access.png)

---

#### Bước 4 — Kích hoạt Mã hóa phía Máy chủ (Server-Side Encryption) và Tạo

Trong phần **Default encryption**:
- **Encryption type**: SSE-S3 (Khóa do Amazon S3 quản lý)

Giữ nguyên tất cả các cài đặt khác ở mức mặc định và nhấp vào **Create bucket**.

![Kích hoạt SSE-S3 và Tạo](/images/4-Workshop/S3_Bucket/B4_Enable_SS_Encryption.png)

---

#### Bước 5 — Mở Bucket và Tạo Thư mục

Nhấp vào bucket mới tạo để mở nó. Nhấp vào **Create folder** để thêm tiền tố đầu tiên.

![Mở Bucket và Tạo Thư mục](/images/4-Workshop/S3_Bucket/B5_Create_Folder_Prefix.png)

---

#### Bước 6 — Tạo Thư mục `raw_audio/`

- **Folder name**: `raw_audio`

Nhấp vào **Create folder**. Lặp lại các bước 5-6 để tạo thêm một thư mục `transcripts/`.

| Thư mục | Mục đích |
|---|---|
| `raw_audio/` | Nhận các tệp âm thanh được tải lên thông qua các URL được ký trước (presigned URLs) |
| `transcripts/` | Nhận đầu ra JSON từ AWS Transcribe |

![Tạo Thư mục raw_audio](/images/4-Workshop/S3_Bucket/B6_Create_Presigned_Prefix.png)

---

{{% notice tip %}}
✅ Bucket S3 Âm thanh của bạn đã sẵn sàng với hai thư mục: `raw_audio/` và `transcripts/`. Hàm Lambda sẽ sử dụng `raw_audio/` làm tiền tố kích hoạt (trigger prefix) và ghi kết quả vào `transcripts/`.
{{% /notice %}}