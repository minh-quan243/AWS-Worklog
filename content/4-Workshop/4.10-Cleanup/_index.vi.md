---
title: "Dọn dẹp tài nguyên"
date: 2026-04-04
weight: 10
chapter: false
pre: " <b> 4.10. </b> "
---

Chúc mừng bạn đã hoàn thành workshop Voice Summarizer! 🎉

Trong workshop này, bạn đã xây dựng một nền tảng trí tuệ âm thanh hoàn chỉnh được hỗ trợ bởi AI trên AWS, bao gồm:
- Một **VPC** với các subnet công khai/riêng (public/private), Internet Gateway, NAT Gateway và Gateway Endpoints
- **Ba bucket S3** cho âm thanh, bản phiên âm và các vector nhúng
- **Hai bảng DynamoDB** cho trạng thái bản ghi âm và dữ liệu người dùng
- **Hai hàm Lambda** để kích hoạt quá trình phiên âm và cấp phát người dùng
- Một **EC2 instance** chạy FastAPI, Celery và Redis
- Một **Application Load Balancer** định tuyến lưu lượng truy cập đến subnet riêng
- Một **Cognito User Pool** với trigger Hậu xác nhận (Post Confirmation)
- Một bản triển khai **Amplify** phân phối frontend React từ GitHub

Để tránh các khoản phí phát sinh, hãy xóa tất cả tài nguyên theo thứ tự bên dưới. **Luôn xóa các tài nguyên trước khi xóa các thành phần mạng mà chúng phụ thuộc vào.**

{{% notice warning %}}
⚠️ **NAT Gateway** (~$43/tháng) và **ALB** (~$18/tháng) và **EC2** (~$15/tháng) là các dịch vụ tốn kém nhất. Nếu bạn đang tạm dừng thay vì kết thúc hoàn toàn workshop, ít nhất hãy **dừng EC2 instance**, **xóa NAT Gateway** và **xóa ALB** để ngừng phần lớn các khoản phí.
{{% /notice %}}

---

### Bước 1 — Xóa Ứng dụng Amplify

Điều hướng đến **AWS Amplify**. Chọn `voice-summarizer` và nhấp vào **Actions → Delete app**. Xác nhận xóa.

Việc này sẽ xóa bản phân phối CDN và tất cả các tệp cấu trúc bản dựng (build artifacts).

---

### Bước 2 — Xóa Cognito User Pool

Điều hướng đến **Amazon Cognito → User pools**. Chọn `voice-summarizer-user-pool`. Nhấp vào **Actions → Delete**. Nhập tên pool để xác nhận và nhấp vào **Delete**.

{{% notice info %}}
Việc xóa User Pool sẽ xóa vĩnh viễn tất cả các tài khoản người dùng. Không thể hoàn tác việc này.
{{% /notice %}}

---

### Bước 3 — Xóa các Hàm Lambda

Điều hướng đến **Lambda → Functions**. Chọn từng hàm và nhấp vào **Actions → Delete**:

- `audio2text`
- `user_creation_db`

Xác nhận xóa cho từng hàm.

---

### Bước 4 — Xóa Application Load Balancer

Điều hướng đến **EC2 → Load Balancing → Load Balancers**. Chọn `voice-summarizer-alb`, nhấp vào **Actions → Delete load balancer**. Xác nhận.

Sau đó điều hướng đến **Target Groups**, chọn `voice-summarizer-tg`, và nhấp vào **Actions → Delete**.

---

### Bước 5 — Chấm dứt (Terminate) EC2 Instance

Điều hướng đến **EC2 → Instances**. Chọn `voice-summarizer-backend`. Nhấp vào **Instance state → Terminate instance**. Xác nhận chấm dứt.

Chờ trạng thái của instance chuyển sang **Terminated** (Đã chấm dứt) trước khi tiếp tục việc dọn dẹp VPC.

---

### Bước 6 — Xóa Route 53 Hosted Zone

Điều hướng đến **Route 53 → Hosted zones**. Nhấp vào hosted zone của bạn (ví dụ: `yourdomain.com`).

Đầu tiên xóa tất cả các bản ghi không phải mặc định bên trong zone:
1. Chọn tất cả các bản ghi **ngoại trừ** bản ghi NS và SOA
2. Nhấp vào **Delete records** và xác nhận

Sau đó, trở lại danh sách Hosted zones, chọn zone và nhấp vào **Delete**. Nhập tên miền để xác nhận.

{{% notice info %}}
Route 53 Hosted Zones tính phí $0.50/tháng. Nếu bạn dự định tái sử dụng tên miền, bạn có thể giữ lại zone và chỉ xóa bản ghi bí danh A (alias A record) — bản thân các bản ghi NS và SOA không bị tính phí riêng.
{{% /notice %}}

---

### Bước 7 — Làm trống và Xóa Bucket S3

Điều hướng đến **S3**. Nhấp vào bucket `one4allthing` (hoặc tên bạn đã chọn):

1. Nhấp vào **Empty** — gõ `permanently delete` và xác nhận. Việc này sẽ xóa tất cả các đối tượng trên cả bốn tiền tố (`raw_audio/`, `transcripts/`, `vectors/`, `summarize/`).
2. Sau khi làm trống, nhấp vào **Delete** — gõ tên bucket và xác nhận.

---

### Bước 8 — Xóa Bảng DynamoDB

Điều hướng đến **DynamoDB → Tables**. Chọn `voice-summarizer-table` (hoặc tên bạn đã chọn) và nhấp vào **Delete**. Xác nhận xóa.

---

### Bước 9 — Xóa NAT Gateway

Điều hướng đến **VPC → NAT Gateways**. Chọn `voice-summarizer-nat`. Nhấp vào **Actions → Delete NAT gateway**. Xác nhận.

{{% notice info %}}
NAT Gateways mất vài phút để xóa hoàn toàn. Đợi cho đến khi trạng thái hiển thị là **Deleted** (Đã xóa) trước khi giải phóng Elastic IP được liên kết.
{{% /notice %}}

Sau khi xóa, điều hướng đến **VPC → Elastic IPs**. Chọn Elastic IP đã được cấp phát cho NAT Gateway (trạng thái sẽ hiển thị là **Available** - Có sẵn). Nhấp vào **Actions → Release Elastic IP address**. Xác nhận.

---

### Bước 10 — Xóa các VPC Endpoints

Điều hướng đến **VPC → Endpoints**. Chọn cả hai endpoint:

- `voice-summarizer-s3-endpoint`
- `voice-summarizer-dynamodb-endpoint`

Nhấp vào **Actions → Delete VPC endpoints**. Xác nhận.

---

### Bước 11 — Xóa Internet Gateway

Điều hướng đến **VPC → Internet Gateways**. Chọn `voice-summarizer-igw`.

Đầu tiên, **tách nó khỏi VPC (detach it from the VPC)**: Nhấp vào **Actions → Detach from VPC**, xác nhận.  
Sau đó, **xóa nó**: Nhấp vào **Actions → Delete internet gateway**, xác nhận.

---

### Bước 12 — Xóa Bảng định tuyến (Route Tables)

Điều hướng đến **VPC → Route Tables**. Chọn `voice-summarizer-private-rt`. Nhấp vào **Actions → Delete route table**. Xác nhận.

Bảng định tuyến chính (main route table) liên kết với VPC sẽ tự động bị xóa khi VPC bị xóa.

---

### Bước 13 — Xóa các Subnet

Điều hướng đến **VPC → Subnets**. Chọn từng subnet và nhấp vào **Actions → Delete subnet**:

- `voice-summarizer-public-subnet`
- `voice-summarizer-private-subnet`

---

### Bước 14 — Xóa Nhóm bảo mật (Security Group)

Điều hướng đến **VPC → Security Groups**. Chọn `voice-summarizer-ec2-sg`. Nhấp vào **Actions → Delete security groups**. Xác nhận.

---

### Bước 15 — Xóa VPC

Điều hướng đến **VPC → Your VPCs**. Chọn `voice-summarizer-vpc`. Nhấp vào **Actions → Delete VPC**. Xác nhận.

---

### Bước 16 — Xóa IAM Roles

Điều hướng đến **IAM → Roles**. Tìm kiếm và xóa các execution roles đã tạo cho:

- Hàm Lambda `audio2text`
- Hàm Lambda `user_creation_db`
- Profile của EC2 instance (EC2 instance profile)

---

### Xác minh Không còn Khoản phí nào

Sau khi hoàn thành tất cả các bước trên, hãy xác minh trong AWS Console:

| Dịch vụ | Trạng thái Dự kiến |
|---|---|
| EC2 Instances | Không có instance nào đang chạy |
| NAT Gateways | Không có gateway nào (trạng thái: Deleted) |
| Load Balancers | Không có load balancer nào đang hoạt động |
| Elastic IPs | Không có IP nào được cấp phát |
| Route 53 Hosted Zones | Zone của workshop đã bị xóa |
| S3 Buckets | Bucket `one4allthing` đã bị xóa |
| DynamoDB Tables | `voice-summarizer-table` đã bị xóa |
| Lambda Functions | Các hàm của workshop đã bị xóa |
| Cognito User Pools | Pool của workshop đã bị xóa |
| Amplify Apps | Ứng dụng của workshop đã bị xóa |

Bạn cũng có thể kiểm tra **AWS Cost Explorer** hoặc **AWS Budgets** để xem có bất kỳ khoản phí tồn đọng nào từ các dịch vụ thanh toán sau hay không.

---

{{% notice tip %}}
✅ Tất cả các tài nguyên của workshop đã được dọn dẹp. Cảm ơn bạn đã hoàn thành Workshop Voice Summarizer trên AWS!
{{% /notice %}}