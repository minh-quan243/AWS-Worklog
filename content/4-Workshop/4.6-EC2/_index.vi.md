---
title: "Khởi chạy EC2 Instance"
date: 2026-04-04
weight: 6
chapter: false
pre: " <b> 4.6. </b> "
---

EC2 instance là nút tính toán cốt lõi của nền tảng. Nó chạy ba tiến trình được đặt cùng vị trí (co-located processes):
- **FastAPI** — máy chủ REST API xử lý việc tạo URL tải lên, các truy vấn Hỏi & Đáp (Q&A), thăm dò trạng thái (status polling) và truy cập thư viện
- **Celery worker** — bộ điều phối luồng xử lý bất đồng bộ (kiểm tra tải lên → chờ Transcribe → vector hóa)
- **Redis** — message broker (trình môi giới tin nhắn) của Celery

Instance này được triển khai trong **subnet riêng (private subnet)**, vì vậy nó không có IP công khai. Tất cả lưu lượng truy cập đến (inbound) đi qua Application Load Balancer (sẽ được thiết lập trong phần tiếp theo), và lưu lượng truy cập đi ra (outbound) tới các LLM API bên ngoài sẽ thoát ra thông qua NAT Gateway.

---

#### Bước 1 — Mở EC2 và Khởi chạy một Instance

Điều hướng đến **EC2** trong AWS Console. Nhấp vào **Launch instance**.

Cấu hình các thông tin cơ bản:
- **Name**: `voice-summarizer-backend`
- **AMI**: Ubuntu Server 24.04 LTS (hoặc Amazon Linux 2023)
- **Instance type**: `t3.xlarge` *(xem lưu ý bên dưới)*

![Khởi chạy EC2 Instance](/images/4-Workshop/EC2/B1_Launch_EC2.png)

{{% notice info %}}
**Kích thước Instance:** Bạn nên sử dụng `t3.xlarge` (4 vCPU, 16 GB RAM) để chạy mô hình nhúng (embedding model) Sentence Transformer cùng với FastAPI và Celery mà không bị áp lực về bộ nhớ. Một instance `t3.medium` (2 vCPU, 4 GB RAM) có thể hoạt động cho các thử nghiệm nhẹ nhưng rất dễ bị lỗi hết bộ nhớ (OOM - Out of Memory) khi tải embedding model.
{{% /notice %}}

---

#### Bước 2 — Chọn Loại Instance

Chọn **t3.xlarge** từ danh sách loại instance (instance type).

![Chọn xlarge](/images/4-Workshop/EC2/B2_Choose_Instance_Type.png)

---

#### Bước 3 — Cấu hình Mạng, Lưu trữ và Khởi chạy

Cuộn xuống để cấu hình các cài đặt còn lại:

**Cặp khóa (Key pair):**
- Tạo một cặp khóa mới hoặc chọn một cặp khóa hiện có. Tải xuống và lưu trữ tệp `.pem` một cách an toàn — bạn sẽ cần nó để SSH vào instance cho việc thiết lập ban đầu.

**Cài đặt mạng (Network settings):**
- **VPC**: `voice-summarizer-vpc`
- **Subnet**: `voice-summarizer-private-subnet`
- **Auto-assign public IP**: **Disable** (Tắt) *(instance này nằm trong subnet riêng)*
- **Security group**: Chọn `voice-summarizer-ec2-sg` đã được tạo ở Phần 3

**Lưu trữ (Storage):**
- Ổ đĩa gốc (Root volume): **30 GB gp3** (tăng lên 50 GB nếu bạn dự định lưu trữ trọng số mô hình lớn cục bộ)

![Cấu hình EC2](/images/4-Workshop/EC2/B3_EC2_Configuration.png)

Nhấp vào **Launch instance**.

---

#### Thiết lập Phần mềm Ban đầu (thông qua SSH bằng Bastion hoặc SSM)

{{% notice info %}}
Vì EC2 instance không có IP công khai, hãy kết nối với nó thông qua **AWS Systems Manager Session Manager** (không yêu cầu khóa SSH, không cần bastion) hoặc bằng cách tạm thời đặt một bastion host trong subnet công khai. Để sử dụng Session Manager, hãy gắn policy `AmazonSSMManagedInstanceCore` vào role của EC2 instance.
{{% /notice %}}

{{% notice info %}}
Bạn cũng có thể gán một IP công khai cho EC2 instance và kết nối với nó qua SSH. Tuy nhiên, cách này không được khuyến nghị cho các môi trường thực tế (production).
{{% /notice %}}

Sau khi kết nối, hãy chạy các lệnh sau để thiết lập môi trường:

```bash
# Cập nhật và cài đặt các phụ thuộc hệ thống
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y python3-pip python3-venv redis-server git

# Khởi động Redis
sudo systemctl enable redis-server
sudo systemctl start redis-server

# Clone dự án
git clone [https://github.com/](https://github.com/)<your-org>/voice-summarizer.git
cd voice-summarizer

# Tạo môi trường ảo và cài đặt các thư viện phụ thuộc
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Tạo tệp .env
cat > .env << EOF
REGION=ap-southeast-1
HISTORY_TABLE=VoiceSummarizerHistory
AUDIO_BUCKET=one4allthing
VECTORS_BUCKET=<your-vectors-bucket-name>
LITELLM_MODEL=claude-sonnet-4-20250514
LITELLM_API_KEY=<your-api-key>
EOF

# Khởi chạy FastAPI (chạy nền)
nohup uvicorn api.main:app --host 0.0.0.0 --port 8000 &

# Khởi chạy Celery worker (chạy nền)
nohup celery -A worker.celery_app worker --loglevel=info &
```

---

#### Gắn một IAM Role vào EC2 Instance

EC2 instance cần có quyền để truy cập S3 và DynamoDB. Điều hướng đến **EC2 → Instances**, chọn instance, và nhấp vào **Actions → Security → Modify IAM role**. Gắn một role với các quyền sau:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:HeadObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::one4allthing",
        "arn:aws:s3:::one4allthing/*",
        "arn:aws:s3:::<your-vectors-bucket>",
        "arn:aws:s3:::<your-vectors-bucket>/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": ["dynamodb:GetItem", "dynamodb:PutItem", "dynamodb:UpdateItem", "dynamodb:Query"],
      "Resource": [
        "arn:aws:dynamodb:ap-southeast-1:*:table/VoiceSummarizerHistory",
        "arn:aws:dynamodb:ap-southeast-1:*:table/User"
      ]
    }
  ]
}
```

---

{{% notice tip %}}
✅ EC2 instance của bạn đang chạy trong subnet riêng với FastAPI trên cổng 8000, Celery và Redis. Trong phần tiếp theo, bạn sẽ tạo một Application Load Balancer để hiển thị cổng 8000 ra bên ngoài cho frontend sử dụng.
{{% /notice %}}