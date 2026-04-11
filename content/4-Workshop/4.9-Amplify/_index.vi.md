---
title: "Triển khai Frontend với AWS Amplify"
date: 2026-04-04
weight: 9
chapter: false
pre: " <b> 4.9. </b> "
---

AWS Amplify lưu trữ và triển khai liên tục frontend React + Vite trực tiếp từ kho lưu trữ GitHub của bạn. Mỗi thao tác đẩy (push) lên nhánh đã được cấu hình sẽ kích hoạt quá trình tự động xây dựng lại (rebuild) và triển khai. Amplify xử lý HTTPS, phân phối CDN và cấu hình tên miền tùy chỉnh.

**Trước khi bắt đầu phần này, hãy đảm bảo:**
- Tệp `aws-config.js` chứa Pool ID và Client ID của Cognito đã được commit vào kho lưu trữ của bạn (Phần 8, Bước 7)
- Tên DNS của ALB đã được thiết lập làm URL cơ sở của API (API base URL) trong `src/services/apiClient.js`

---

#### Bước 1 — Mở AWS Amplify và Kết nối với GitHub

Điều hướng đến **AWS Amplify** trong AWS Console. Nhấp vào **Create new app** (Tạo ứng dụng mới), sau đó chọn **GitHub** làm nguồn (source).

Cấp quyền cho Amplify truy cập tài khoản GitHub của bạn nếu được yêu cầu.

![Mở Amplify và Chọn GitHub](/images/4-Workshop/Amplify/B1_Open_Amplify_Choose_Github.png)

---

#### Bước 2 — Chọn Kho lưu trữ (Repository)

Từ danh sách kho lưu trữ, hãy chọn kho chứa mã nguồn frontend của Voice Summarizer (`voice-summarizer` hoặc tương đương). Nếu kho lưu trữ không xuất hiện, hãy nhấp vào **View GitHub permissions** (Xem quyền GitHub) và cấp cho Amplify quyền truy cập vào đúng kho lưu trữ.

![Chọn Kho lưu trữ](/images/4-Workshop/Amplify/B2_Select_Repo.png)

---

#### Bước 3 — Chọn Nhánh (Branch) và Thư mục gốc (Root Directory)

- **Branch**: `main` (hoặc nhánh triển khai của bạn)
- **App root directory**: `api/fe` *(frontend React nằm bên trong thư mục con `api/fe/` của monorepo)*

Amplify sẽ tự động phát hiện các cài đặt bản dựng Vite. Hãy xác nhận lệnh build và thư mục đầu ra:

| Cài đặt | Giá trị |
|---|---|
| Build command (Lệnh build) | `npm run build` |
| Build output directory (Thư mục đầu ra) | `dist` |
| Node.js version | 18 hoặc 20 |

![Chọn Nhánh và Thư mục gốc](/images/4-Workshop/Amplify/B3_Choose_Branch_And_Root_Dir.png)

---

#### Bước 4 — Xem lại và Tạo Ứng dụng Amplify

Xem lại tất cả các cài đặt. Thêm bất kỳ **biến môi trường** (environment variables) bắt buộc nào mà bản dựng frontend của bạn cần (ví dụ: `VITE_API_URL` trỏ đến tên DNS của ALB nếu bạn sử dụng nó tại thời điểm build).

Nhấp vào **Save and deploy** (Lưu và triển khai).

Amplify sẽ cấp phát một môi trường build, clone kho lưu trữ, chạy quá trình build và triển khai đầu ra lên CDN của nó. Quá trình này thường mất từ 2-4 phút.

![Tạo Ứng dụng Amplify](/images/4-Workshop/Amplify/B4_Review_And_Create.png)

---

#### Bước 5 — Chờ Triển khai và Mở Tên miền

Khi quy trình triển khai hiển thị tất cả các dấu tích màu xanh lá cây (Provision → Build → Deploy → Verify), hãy nhấp vào liên kết **Domain** (Tên miền) để mở ứng dụng trực tiếp.

Định dạng tên miền mặc định là:
```
https://main.<app-id>.amplifyapp.com
```

![Mở Tên miền Amplify](/images/4-Workshop/Amplify/B5_Wait_For_Deployment.png)

---

#### Xác minh Toàn bộ Hệ thống (Full Stack) Đang hoạt động

Mở URL trực tiếp và kiểm tra các tính năng sau:

1. **Đăng ký** — Tạo một tài khoản người dùng mới. Bạn sẽ nhận được một email xác minh từ Cognito.
2. **Xác minh email** — Nhấp vào liên kết xác minh. Lambda `user_creation_db` sẽ được kích hoạt và tạo một bản ghi DynamoDB trong bảng `User`.
3. **Đăng nhập** — Đăng nhập bằng tài khoản mới. Cognito sẽ cấp phát một token JWT và lưu trữ trong phiên làm việc của trình duyệt.
4. **Tải lên âm thanh** — Tải lên một tệp âm thanh ngắn. Tệp này sẽ được đưa vào `s3://one4allthing/raw_audio/`, kích hoạt Lambda `audio2text`, bắt đầu một công việc Transcribe và cuối cùng sẽ hiển thị trạng thái `completed` (hoàn tất) trên bảng điều khiển.
5. **Hỏi & Đáp (Q&A)** — Mở bản ghi âm đã hoàn tất và đặt câu hỏi. Máy chủ FastAPI sẽ trả về một câu trả lời có cơ sở.

{{% notice info %}}
**Tên miền tùy chỉnh:** Để sử dụng tên miền tùy chỉnh thay vì URL `.amplifyapp.com` mặc định, hãy đi tới **Amplify → App settings → Domain management** (Quản lý tên miền) và thêm tên miền của bạn. Amplify sẽ tự động cấp phát chứng chỉ SSL thông qua ACM.
{{% /notice %}}

---

{{% notice tip %}}
✅ Nền tảng Voice Summarizer đã được triển khai hoàn tất! Toàn bộ luồng dữ liệu hiện đã hoạt động (live):
Người dùng → Amplify → Cognito (xác thực) → ALB → EC2 (FastAPI + Celery) → S3 / DynamoDB / Transcribe / Lambda → LLM API
{{% /notice %}}