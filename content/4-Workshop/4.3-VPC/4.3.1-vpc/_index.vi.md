---
title: "VPC Setup"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.3.1. </b> "
---

Trong phần này, bạn sẽ tạo toàn bộ nền tảng mạng cho nền tảng Voice Summarizer: một VPC với các subnet công khai (public) và riêng (private), một Internet Gateway cho truy cập đến (inbound), một NAT Gateway để EC2 instance trong mạng riêng có thể kết nối với các API LLM bên ngoài, các VPC Gateway Endpoints để EC2 có thể truy cập S3 và DynamoDB mà không cần đi qua internet, và một Nhóm bảo mật (Security Group) để bảo vệ EC2 instance.

{{% notice info %}}
Tất cả các tài nguyên trong phần này phải được tạo ở **ap-southeast-1 (Singapore)**.
{{% /notice %}}

---

#### Bước 1 — Tạo VPC

Điều hướng đến **VPC** trong AWS Console. Ở thanh bên trái, nhấp vào **Your VPCs**, sau đó nhấp vào **Create VPC**.

![Tạo VPC](/images/4-Workshop/VPC/B1_Create_VPC.png)

---

#### Bước 2 — Cấu hình VPC

Điền các cài đặt VPC như được hiển thị:

- **Name tag**: `voice-summarizer-vpc`
- **IPv4 CIDR**: `10.0.0.0/16`
- Giữ nguyên tất cả các cài đặt khác ở mức mặc định

Nhấp vào **Create VPC**.

![Cấu hình VPC](/images/4-Workshop/VPC/B2_Configure_VPC.png)

---

#### Bước 3 — Tạo các Subnet

Ở thanh bên trái, nhấp vào **Subnets**, sau đó nhấp vào **Create subnet**. Chọn VPC bạn vừa tạo.

Bạn sẽ tạo **hai subnet** — một subnet công khai (cho ALB) và một subnet riêng (cho EC2):

![Tạo Subnet](/images/4-Workshop/VPC/B3_Create_Subnet.png)

---

#### Bước 4 — Cấu hình các Subnet

Cấu hình các subnet như sau:

**Public Subnet (cho ALB):**
- **Name**: `voice-summarizer-public-subnet`
- **Availability Zone**: `ap-southeast-1a`
- **IPv4 CIDR**: `10.0.1.0/24`

**Private Subnet (cho EC2):**
- **Name**: `voice-summarizer-private-subnet`
- **Availability Zone**: `ap-southeast-1a`
- **IPv4 CIDR**: `10.0.2.0/24`

Nhấp vào **Create subnet**.

![Cấu hình Subnet](/images/4-Workshop/VPC/B4_Configure_Subnet.png)

---

#### Bước 5 — Điều hướng đến Internet Gateways

Ở thanh bên trái, nhấp vào **Internet Gateways**, sau đó nhấp vào **Create internet gateway**.

![Menu Internet Gateway](/images/4-Workshop/VPC/B5_Create_IGW.png)

---

#### Bước 6 — Tạo Internet Gateway

- **Name tag**: `voice-summarizer-igw`

Nhấp vào **Create internet gateway**.

![Tạo IGW](/images/4-Workshop/VPC/B6_Create_IGW.png)

---

#### Bước 7 — Gắn Internet Gateway vào VPC

Sau khi tạo xong, chọn IGW mới và nhấp vào **Actions → Attach to VPC**. Chọn `voice-summarizer-vpc` và xác nhận.

![Gắn IGW vào VPC](/images/4-Workshop/VPC/B7_Attach_IGW_VPC.png)

---

#### Bước 8 — Tạo NAT Gateway

Ở thanh bên trái, nhấp vào **NAT Gateways**, sau đó nhấp vào **Create NAT gateway**.

- **Name**: `voice-summarizer-nat`
- **Subnet**: `voice-summarizer-public-subnet` *(NAT Gateway phải nằm trong subnet **public**)*
- **Connectivity type**: Public
- Nhấp vào **Allocate Elastic IP**, sau đó nhấp vào **Create NAT gateway**

{{% notice warning %}}
⚠️ NAT Gateway là dịch vụ tốn kém nhất trong workshop này (~$43/tháng). Hãy nhớ xóa nó trong quá trình dọn dẹp khi workshop hoàn tất.
{{% /notice %}}

![Tạo NAT Gateway](/images/4-Workshop/VPC/B8_Create_NAT.png)

---

#### Bước 9 — Tạo Bảng định tuyến riêng (Private Route Table)

Ở thanh bên trái, nhấp vào **Route Tables**, sau đó nhấp vào **Create route table**.

- **Name**: `voice-summarizer-private-rt`
- **VPC**: `voice-summarizer-vpc`

Nhấp vào **Create route table**.

![Tạo Private Route](/images/4-Workshop/VPC/B9_Create_Private_Route.png)

---

#### Bước 10 — Xác nhận Bảng định tuyến riêng

Xác minh rằng bảng định tuyến đã được tạo và được liên kết với đúng VPC.

![Bảng Private Route đã được tạo](/images/4-Workshop/VPC/B10_Create_Private_RTB.png)

---

#### Bước 11 — Chỉnh sửa các Tuyến đường (Routes) để Thêm NAT Gateway

Chọn `voice-summarizer-private-rt`, nhấp vào tab **Routes**, sau đó nhấp vào **Edit routes**.

Thêm một tuyến đường mới:
- **Destination**: `0.0.0.0/0`
- **Target**: Chọn **NAT Gateway** → `voice-summarizer-nat`

Nhấp vào **Save changes**. Sau đó đi tới **Subnet associations** và liên kết bảng định tuyến này với `voice-summarizer-private-subnet`.

![Chỉnh sửa Routes](/images/4-Workshop/VPC/B11_Edit_RTB.png)

---

#### Bước 12 — Tạo VPC Gateway Endpoints cho S3 và DynamoDB

Gateway Endpoints cho phép EC2 instance trong subnet riêng truy cập S3 và DynamoDB mà không cần đi qua NAT Gateway, giúp cải thiện cả tính bảo mật và chi phí.

Ở thanh bên trái, nhấp vào **Endpoints**, sau đó nhấp vào **Create endpoint**.

Tạo **hai endpoint** (lặp lại thao tác cho mỗi endpoint):

**Endpoint 1 — S3:**
- **Name**: `voice-summarizer-s3-endpoint`
- **Service category**: AWS services
- **Service**: Tìm kiếm `com.amazonaws.ap-southeast-1.s3` → chọn loại **Gateway**
- **VPC**: `voice-summarizer-vpc`
- **Route tables**: Chọn `voice-summarizer-private-rt`

**Endpoint 2 — DynamoDB:**
- **Name**: `voice-summarizer-dynamodb-endpoint`
- **Service**: `com.amazonaws.ap-southeast-1.dynamodb` → loại **Gateway**
- **VPC**: `voice-summarizer-vpc`
- **Route tables**: Chọn `voice-summarizer-private-rt`

![Tạo Gateway Endpoints](/images/4-Workshop/VPC/B12_Create_Endpoint_DynamoDB_S3.png)

---

#### Bước 13 — Tạo Nhóm bảo mật (Security Group) cho EC2

Ở thanh bên trái, nhấp vào **Security Groups**, sau đó nhấp vào **Create security group**.

- **Name**: `voice-summarizer-ec2-sg`
- **Description**: Security group for Voice Summarizer EC2
- **VPC**: `voice-summarizer-vpc`

**Quy tắc Inbound (chiều đến):**

| Type | Protocol | Port | Source | Description |
|---|---|---|---|---|
| Custom TCP | TCP | 8000 | ALB Security Group (hoặc `10.0.0.0/16`) | FastAPI |
| SSH | TCP | 22 | IP của bạn | Quyền truy cập Admin |

**Quy tắc Outbound (chiều đi):** Cho phép tất cả lưu lượng (mặc định).

Nhấp vào **Create security group**.

![Tạo Security Group](/images/4-Workshop/VPC/B13_Create_SG.png)

---

#### Bước 14 — Thêm Tuyến đường cho Truy cập LLM API bên ngoài (Bedrock/NAT)

Nếu bạn đang định tuyến các lời gọi LLM đến AWS Bedrock, hãy thêm một tuyến đường trong bảng định tuyến riêng để đảm bảo lưu lượng được hướng đi chính xác.

Điều hướng trở lại **Route Tables → voice-summarizer-private-rt → Routes → Edit routes** và xác nhận rằng tuyến đường NAT Gateway `0.0.0.0/0` đã có sẵn và đang hoạt động.

![Tuyến đường Bedrock NAT](/images/4-Workshop/VPC/B14_Add_Route_LLM_API.png)

---

#### Bước 15 — Xác minh Cấu hình Kết nối Bedrock / LLM bên ngoài

Xác nhận cấu hình định tuyến đã hoàn tất: bảng định tuyến của subnet riêng có `0.0.0.0/0 → NAT Gateway` cho các lời gọi LLM API hướng ra internet và Gateway Endpoints xử lý lưu lượng S3, DynamoDB một cách riêng tư.

![Xác minh Cấu hình Bedrock](/images/4-Workshop/VPC/B15_Verify_Connection.png)

---

#### Bước 16 — Gán Subnet công khai cho ALB

Điều hướng đến **Subnets**, chọn `voice-summarizer-public-subnet`, và nhấp vào **Actions → Edit subnet settings**. Kích hoạt **Auto-assign public IPv4 address** (Tự động gán địa chỉ IPv4 công khai). Điều này đảm bảo ALB (sẽ được triển khai trong phần tiếp theo) có thể nhận được lưu lượng truy cập công khai.

![Cài đặt Subnet ALB](/images/4-Workshop/VPC/B16_Assign_PubSubnet_ALB.png)

---

{{% notice tip %}}
✅ Bạn đã hoàn tất việc thiết lập mạng. VPC của bạn hiện có:<br>• Một **subnet công khai** với quyền truy cập internet thông qua Internet Gateway (dành cho ALB)<br>• Một **subnet riêng** với quyền truy cập chỉ chiều đi ra thông qua NAT Gateway (dành cho EC2)<br>• Các **Gateway Endpoints** để truy cập riêng tư vào S3 và DynamoDB<br>• Một **security group** bảo vệ EC2 instance
{{% /notice %}}