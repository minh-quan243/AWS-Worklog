---
title: "Tích hợp Application Load Balancer"
date: 2026-04-04
weight: 7
chapter: false
pre: " <b> 4.7. </b> "
---

Application Load Balancer (ALB) nằm trong **subnet công khai (public subnet)** và đóng vai trò là điểm truy cập duy nhất cho tất cả lưu lượng API từ frontend Amplify. Nó chuyển tiếp các yêu cầu đến EC2 instance trên cổng 8000 trong subnet riêng (private subnet) thông qua một Target Group.

Việc thiết lập gồm hai phần:
1. Tạo một **Target Group** (Nhóm đích) để đăng ký EC2 instance
2. Tạo **ALB** và gắn Target Group vào một listener (trình lắng nghe)

---

### Phần 1 — Tạo Target Group

#### Bước 1 — Mở EC2 và Điều hướng đến Target Groups

Điều hướng đến **EC2** trong AWS Console. Ở thanh bên trái, cuộn xuống phần **Load Balancing → Target Groups**. Nhấp vào **Create target group**.

Cấu hình target group:
- **Target type**: Instances
- **Target group name**: `voice-summarizer-tg`
- **Protocol**: HTTP
- **Port**: `8000`
- **VPC**: `voice-summarizer-vpc`
- **Health check protocol**: HTTP
- **Health check path**: `/` *(hoặc `/health` nếu ứng dụng FastAPI của bạn có endpoint kiểm tra sức khỏe)*

![Cài đặt Tạo Target Group](/images/4-Workshop/ALB/B1_Create_TargetGroup.png)

---

#### Bước 2 — Đăng ký EC2 Instance

Cuộn xuống phần **Register targets**. Chọn EC2 instance `voice-summarizer-backend` và nhấp vào **Include as pending below**.

![Chọn EC2 Instance](/images/4-Workshop/ALB/B2_Register_EC2_Ins.png)

---

#### Bước 3 — Xem lại trước khi Tạo

Xem lại tất cả các cài đặt — xác nhận rằng đối tượng đích (target) là đúng EC2 instance trên cổng 8000 và VPC là chính xác.

![Xem lại Target Group](/images/4-Workshop/ALB/B2_Review_Info.png)

---

#### Bước 4 — Tạo Target Group

Nhấp vào **Create target group**. Đợi trạng thái của target group hiển thị là **Active** và tình trạng sức khỏe (health) của mục tiêu cuối cùng sẽ hiển thị là **Healthy** sau khi ALB bắt đầu gửi các yêu cầu kiểm tra sức khỏe.

![Tạo Target Group](/images/4-Workshop/ALB/B4_Create_TargetGroup.png)

---

### Phần 2 — Tạo Application Load Balancer

#### Bước 5 — Điều hướng đến Load Balancers và chọn ALB

Ở thanh bên trái, nhấp vào **Load Balancers**. Nhấp vào **Create load balancer**. Trên trang lựa chọn, chọn **Application Load Balancer**.

![Chọn Application Load Balancer](/images/4-Workshop/ALB/B5_Choose_ALB.png)

---

#### Bước 6 — Cấu hình ALB — Các cài đặt Cơ bản

Điền thông tin cấu hình ALB:

- **Name**: `voice-summarizer-alb`
- **Scheme**: Internet-facing *(cho phép frontend Amplify tiếp cận từ internet công cộng)*
- **IP address type**: IPv4
- **VPC**: `voice-summarizer-vpc`
- **Mappings**: Chọn **ap-southeast-1a** và chọn `voice-summarizer-public-subnet`

{{% notice info %}}
ALB phải được đặt trong **subnet công khai**. Nó nhận lưu lượng từ internet và chuyển tiếp vào bên trong tới EC2 instance nằm trong subnet riêng.
{{% /notice %}}

![Cài đặt Cơ bản ALB](/images/4-Workshop/ALB/B6_Configure_ALB.png)

---

#### Bước 7 — Cấu hình Listener, Security Group và Tạo

**Security groups:**
- Tạo hoặc chọn một security group cho ALB cho phép truy cập inbound **HTTP (80)** và **HTTPS (443)** từ `0.0.0.0/0`

**Listeners and routing:**
- **Protocol**: HTTP
- **Port**: 80
- **Default action**: Forward to `voice-summarizer-tg`

Xem lại tất cả các cài đặt và nhấp vào **Create load balancer**.

![Listener ALB và các Cài đặt Cuối cùng](/images/4-Workshop/ALB/B7_Follow_Configuration.png)

Sau khi tạo xong, hãy lưu lại **DNS name** của ALB (ví dụ: `voice-summarizer-alb-xxxxxxxx.ap-southeast-1.elb.amazonaws.com`). Đây là URL gốc mà frontend của bạn sẽ gọi cho tất cả các yêu cầu API.

---

{{% notice tip %}}
✅ ALB đã được triển khai trong subnet công khai và chuyển tiếp lưu lượng đến EC2 instance.
Hãy sao chép **ALB DNS name** — bạn sẽ cần nó khi cấu hình API client cho frontend trong bước triển khai Amplify.
{{% /notice %}}
