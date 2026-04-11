---
title: "Khởi tạo Cognito User Pool"
date: 2026-04-04
weight: 8
chapter: false
pre: " <b> 4.8. </b> "
---

Amazon Cognito quản lý việc đăng ký người dùng, xác minh email, đăng nhập và cấp phát token JWT cho nền tảng. Frontend React sử dụng **Hosted UI** của Cognito và Amplify/Cognito JS SDK để xác thực người dùng. Một **Lambda trigger hậu xác nhận (Post Confirmation)** kết nối Cognito với Lambda `user_creation_db` từ Phần 5.2, tự động cấp phát một bản ghi DynamoDB khi người dùng mới xác minh email của họ.

Vào cuối phần này, bạn sẽ có:
- Một **User Pool** (Nhóm người dùng) của Cognito
- Một **App Client** (Ứng dụng khách) được cấu hình cho SPA (không có client secret)
- Các giá trị **Pool ID** và **Client ID** cần thiết cho tệp cấu hình frontend

---

#### Bước 1 — Mở Cognito và Đi tới User Pools

Điều hướng đến **Amazon Cognito** trong AWS Console. Nhấp vào **User pools** ở thanh bên trái, sau đó nhấp vào **Create user pool**.

![Mở Cognito User Pools](/images/4-Workshop/Cognito/B1_Navigate_Cognito_Userpool.png)

---

#### Bước 2 — Chọn Loại Ứng dụng: Single-Page Application (SPA)

Trên màn hình đầu tiên, chọn **Single-page application (SPA)** làm loại ứng dụng. Việc này cấu hình app client mà không có client secret (bí mật máy khách), điều này là bắt buộc đối với các luồng xác thực dựa trên trình duyệt.

![Chọn SPA](/images/4-Workshop/Cognito/B2_Choose_SPA.png)

---

#### Bước 3 — Cấu hình Đăng nhập, Tự Đăng ký và Các Thuộc tính

Cấu hình các tùy chọn đăng nhập và đăng ký:

- **Tùy chọn đăng nhập (Sign-in options)**: ✅ **Email**
- **Tự đăng ký (Self-registration)**: ✅ **Enable self-registration** *(cho phép người dùng tự đăng ký)*
- **Các thuộc tính bắt buộc (Required attributes)**: ✅ **email**
- **MFA (Xác thực đa yếu tố)**: Tùy chọn (để tắt cho workshop này)
- **Khôi phục tài khoản người dùng (User account recovery)**: Email

![Cấu hình Email và Tự Đăng ký](/images/4-Workshop/Cognito/B3_Configure_Auth.png)

---

#### Bước 4 — Tạo User Pool

Xem lại tất cả các cài đặt. Cuộn xuống và nhấp vào **Create user pool**.

- **Tên User pool (User pool name)**: `voice-summarizer-user-pool`

![Tạo User Pool](/images/4-Workshop/Cognito/B4_Review_Configuration.png)

---

#### Bước 5 — Sao chép User Pool ID

Sau khi tạo, nhấp vào `voice-summarizer-user-pool` để mở trang chi tiết của nó. Ở trên cùng của tab **Overview** (Tổng quan), tìm và sao chép **User pool ID** (định dạng: `ap-southeast-1_XXXXXXXXX`).

Bạn sẽ dán giá trị này vào tệp cấu hình frontend ở Bước 7.

![Sao chép User Pool ID](/images/4-Workshop/Cognito/B5_Copy_Created_UPID.png)

---

#### Bước 6 — Mở App Client và Sao chép Client ID

Ở thanh bên trái (bên trong chế độ xem chi tiết của User Pool), nhấp vào **App clients**. Nhấp vào app client đã được tự động tạo. Sao chép **Client ID**.

Bạn sẽ dán giá trị này cùng với Pool ID vào tệp cấu hình frontend.

![Sao chép Client ID](/images/4-Workshop/Cognito/B6_Copy_ClientID.png)

---

✅ Cognito đã được cấu hình đầy đủ.
- Người dùng có thể tự đăng ký bằng địa chỉ email của họ
- Yêu cầu xác minh email trước khi được cấp quyền truy cập
- Khi xác minh thành công, Lambda `user_creation_db` sẽ tự động cấp phát một bản ghi DynamoDB
- Tệp `aws-config.js` ở frontend đã có Pool ID và Client ID cần thiết cho Cognito JS SDK

{{% notice tip %}}
Hãy giữ sẵn **User Pool ID** và **Client ID** — bạn sẽ xác minh xem chúng có chính xác hay không trong quá trình kiểm tra triển khai Amplify.
{{% /notice %}}
