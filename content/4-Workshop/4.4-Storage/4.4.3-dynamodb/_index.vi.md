---
title : "DynamoDB Tables"
date: "2000-01-01"
weight: 3
chapter : false
pre : " <b> 4.4.3. </b> "
---

Bạn sẽ tạo **ba bảng DynamoDB**:

| Tên Bảng | Khóa phân vùng (Partition Key) | Khóa sắp xếp (Sort Key) | Mục đích |
|---|---|---|---|
| `voice_status_table` | `raw_id` (String) | — | Trạng thái, tiến độ và giai đoạn của từng bản ghi âm được theo dõi bởi Celery worker |
| `memory_AI` | `raw_id` (String) | — | Lịch sử trò chuyện và bộ nhớ hội thoại cho mỗi phiên ghi âm |
| `User` | `user_id` (String) | `raw_id` (String) | Các bản ghi người dùng được cấp phát bởi trigger Lambda của Cognito |

---

### Bảng 1 & 2 — voice_status_table và memory_AI

Hai bảng này có chung khóa phân vùng (`raw_id`) và được tạo bằng các bước giống hệt nhau. `voice_status_table` theo dõi trạng thái luồng xử lý của từng bản ghi âm (trạng thái, phần trăm tiến độ, nhãn giai đoạn và bản tóm tắt ngắn). `memory_AI` lưu trữ lịch sử cuộc trò chuyện đang diễn ra và bộ nhớ đã nén cho các phiên trò chuyện AI. Cả hai bảng đều được đọc và ghi bởi EC2 Celery worker và các router FastAPI.

#### Bước 1 — Mở DynamoDB và Nhấp Create Table

Điều hướng đến **DynamoDB** trong AWS Console. Nhấp vào **Create table**.

![Nhấp Create Table](/images/4-Workshop/DynamoDB/B1_Click_Create_Table.png)

---

#### Bước 2 — Cấu hình voice_status_table

Điền các cài đặt bảng cho bảng đầu tiên:

- **Table name**: `voice_status_table`
- **Partition key**: `raw_id` — kiểu **String**
- **Sort key**: *(để trống)*
- **Table settings**: Các cài đặt mặc định (Default settings - On-demand capacity)

![Nhập Tên Bảng và Khóa phân vùng](/images/4-Workshop/DynamoDB/B2_Configure_VST.png)

---

#### Bước 3 — Tạo voice_status_table

Xem lại các cài đặt và nhấp vào **Create table**. Đợi cho đến khi trạng thái chuyển sang **Active** (Hoạt động) trước khi tiếp tục.

![Nhấp Create](/images/4-Workshop/DynamoDB/B3_Create_Table.png)

---

#### Bước 4 — Lặp lại cho memory_AI

Nhấp vào **Create table** một lần nữa. Sử dụng cùng cấu hình với tên mới:

- **Table name**: `memory_AI`
- **Partition key**: `raw_id` — kiểu **String**
- **Sort key**: *(để trống)*
- **Table settings**: Các cài đặt mặc định (Default settings - On-demand capacity)

![Cấu hình Bảng memory_AI](/images/4-Workshop/DynamoDB/B4_Configure_MA.png)

Nhấp vào **Create table** và đợi trạng thái chuyển thành **Active**.

---

### Bảng 3 — User

Bảng này được ghi bởi Lambda hậu xác nhận (post-confirmation) của Cognito (`lambda_user_creation_db.py`). Khi người dùng xác minh email của họ sau khi đăng ký, một bản ghi sẽ tự động được tạo tại đây với `user_id`, `email`, `created_at` (thời gian tạo), và các bộ đếm mức sử dụng. Khác với hai bảng đầu tiên, bảng `User` có cả khóa phân vùng (partition key) và khóa sắp xếp (sort key).

#### Bước 5 — Cấu hình Bảng User

Nhấp vào **Create table** một lần nữa. Cấu hình:

- **Table name**: `User`
- **Partition key**: `user_id` — kiểu **String**
- **Sort key**: `raw_id` — kiểu **String**
- **Table settings**: Các cài đặt mặc định (Default settings - On-demand capacity)

![Cấu hình Bảng User](/images/4-Workshop/DynamoDB/B5_Configure_User.png)

---

#### Bước 6 — Xác minh Cả ba Bảng đều Tồn tại

Trong console của DynamoDB, xác nhận rằng cả ba bảng đều được liệt kê với trạng thái **Active**:

![Xác minh Tất cả các Bảng](/images/4-Workshop/DynamoDB/B6_Final_Check.png)

---

{{% notice tip %}}
✅ Cả ba bảng DynamoDB đã sẵn sàng: <br> - **voice_status_table** — theo dõi trạng thái luồng xử lý bản ghi âm (trạng thái, tiến độ, giai đoạn) <br> - **memory_AI** — lưu trữ lịch sử trò chuyện AI và bộ nhớ hội thoại <br> - **User** — lưu trữ các bản ghi người dùng được tạo khi đăng ký Cognito <br> Thiết lập các biến môi trường tương ứng trên EC2 instance: <br> - `STATUS_TABLE=voice_status_table` <br> - `MEMORY_TABLE=memory_AI` <br> - `USERS_TABLE` trong mã Lambda ánh xạ tới `User`
{{% /notice %}}