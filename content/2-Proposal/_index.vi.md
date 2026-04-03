---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

![AWS Logo](/images/2-Proposal/AWSLogo.png)

# Proposal Link: [Proposal](https://docs.google.com/document/d/1Y2f0fEwtpavgm4-i7ZciMh5AVfeVkyjh/edit)

# Voice Summarizer — The Archivist
## AI-Powered Audio Intelligence Platform

**Team Members:**

| Full Name             | Student ID | Role        |
| --------------------- | ---------- | ----------- |
| Phan Minh Nhật Trường | SE196744   | Team Leader |
| Nguyễn Minh Quân      | SE193488   | Member      |
| Phạm Đăng Khoa        | SE192211   | Member      |
| Nguyễn Bá Tùng Dương  | SE193510   | Member      |
| Trịnh Thanh An        | SE193956   | Member      |

**AWS Services:** `Amplify` · `Cognito` · `Lambda` · `S3` · `DynamoDB` · `Transcribe` · `EC2`

---

### 1. Tóm tắt điều hành

Voice Summarizer là một nền tảng AI toàn diện (end-to-end) giúp chuyển đổi các bản ghi âm — cuộc họp, bài giảng, phỏng vấn và cuộc gọi — thành các cơ sở dữ liệu tri thức có thể tìm kiếm và truy vấn. Người dùng tải lên một tệp âm thanh (WAV, MP3, M4A); hệ thống sẽ tự động gỡ băng (transcribe), lập chỉ mục ngữ nghĩa, và cung cấp một giao diện AI đàm thoại để người dùng có thể đặt câu hỏi bằng ngôn ngữ tự nhiên và nhận được các câu trả lời chính xác, bám sát ngữ cảnh.

Nền tảng này được xây dựng trên một kiến trúc AWS lai (hybrid) được triển khai tại khu vực Singapore (ap-southeast-1). Frontend được lưu trữ trên AWS Amplify; backend (FastAPI + Celery + Redis) chạy trên một phiên bản EC2 trong mạng con dùng riêng (private subnet), được đứng trước bởi một Bộ cân bằng tải ứng dụng (Application Load Balancer - ALB) nằm trong mạng con công cộng (public subnet). Một NAT Gateway cung cấp cho phiên bản EC2 quyền truy cập internet ra bên ngoài để thực hiện các lệnh gọi API LLM ngoại vi. AWS Transcribe xử lý việc chuyển đổi giọng nói thành văn bản, được kích hoạt bởi một hàm Lambda khi có đối tượng S3 được tạo; một hàm Lambda thứ hai sẽ cung cấp các bản ghi người dùng DynamoDB khi đăng ký qua Cognito. Ba bucket S3 chuyên dụng lưu trữ âm thanh thô, bản chép lời và dữ liệu vector. Các Điểm cuối Cổng VPC (VPC Gateway Endpoints) kết nối mạng con dùng riêng với S3 và DynamoDB mà không cần đi qua internet công cộng. Các lệnh gọi LLM được định tuyến qua LiteLLM để mang lại sự linh hoạt không phụ thuộc vào mô hình (model-agnostic), và việc truy cập được bảo mật thông qua Amazon Cognito với xác thực bằng JWT.

---

### 2. Tuyên bố vấn đề

#### Vấn đề là gì?

Mỗi năm, các tổ chức thực hiện hàng ngàn giờ ghi âm cuộc họp, cuộc gọi với khách hàng, các buổi đào tạo và phỏng vấn. Bất chấp khối lượng này, thông tin bên trong những bản ghi âm đó thực chất là dữ liệu tối (dark data) — rất khó để tìm kiếm, tóm tắt hoặc hành động dựa trên đó mà không phải nghe từ đầu đến cuối. Những điểm hạn chế hiện tại bao gồm:

- Không có cách nào nhanh chóng để xác định vị trí một quyết định, số liệu, hoặc cái tên cụ thể được nhắc đến trong một bản ghi âm dài
- Việc ghi chép thủ công không nhất quán, thiếu sót và tốn thời gian
- Việc chia sẻ thông tin chi tiết từ các bản ghi yêu cầu phải nghe lại và tóm tắt thủ công
- Các trường hợp sử dụng cho việc tuân thủ và kiểm toán yêu cầu phải có bằng chứng truy xuất được từ các phiên đã ghi âm

#### Giải pháp

Voice Summarizer giải quyết những vấn đề này bằng cách tự động hóa toàn bộ quy trình từ âm thanh gốc đến một giao diện tri thức tương tác được hỗ trợ bởi AI. Nền tảng cung cấp ba khả năng cốt lõi:

1. **Quy trình Âm thanh-thành-Tri thức Tự động** — người dùng tải tệp âm thanh lên thông qua một URL presigned của S3; một worker Celery bất đồng bộ sẽ gỡ băng thông qua AWS Transcribe (được kích hoạt bởi một hàm Lambda), sau đó phân mảnh (chunk), tạo nhúng (embed), và lập chỉ mục bản chép lời vào một kho lưu trữ vector ngữ nghĩa — tất cả đều không cần sự can thiệp của người dùng.
2. **Trợ lý AI Đàm thoại** — mỗi bản ghi âm có giao diện trò chuyện riêng được hỗ trợ bởi một bộ định tuyến câu hỏi RAG nhằm phân loại ý định truy vấn và chọn chiến lược truy xuất tối ưu (thực tế, toàn diện, tóm tắt chủ đề, tóm tắt toàn bộ hoặc ngoài chủ đề), tạo ra các câu trả lời chống ảo giác (hallucination-resistant) dựa trên bản gỡ băng.
3. **Thư viện & Bảng điều khiển Bản ghi** — một thư viện có thể tìm kiếm liệt kê tất cả các bản ghi đã xử lý cùng với các chỉ báo trạng thái, tóm tắt ngắn do AI tạo, thời lượng và ngày tạo.

#### Lợi ích và tỷ suất hoàn vốn (ROI)

Voice Summarizer loại bỏ gánh nặng thủ công của việc phát lại, ghi chép và tóm tắt các bản ghi âm. Các lợi ích chính bao gồm:

- **Tăng năng suất ngay lập tức** — bất kỳ sự kiện, quyết định hoặc mục hành động nào từ bất kỳ bản ghi âm nào đều có thể truy xuất trong vài giây
- **Giảm rủi ro tuân thủ** — toàn bộ bản chép lời và dấu vết kiểm toán được lưu trữ và có thể truy vấn
- **Giữ lại tri thức** — kiến thức nội bộ của tổ chức được ghi lại trong các cuộc họp không còn bị mất đi sau khi kết thúc
- **Chi phí cơ sở hạ tầng có thể dự đoán được** — một phiên bản EC2 với kích thước cố định cung cấp dung lượng tính toán ổn định, với các chi phí biến đổi chỉ giới hạn ở mức sử dụng Transcribe và truyền tải dữ liệu

---

### 3. Kiến trúc giải pháp

Hệ thống được triển khai tại AWS ap-southeast-1 (Singapore) và bao gồm tám lớp liên kết lỏng lẻo (loosely coupled):

![AWS Architecture](/images/2-Proposal/AWSWorkshopArchitecture-Final.png)

| Lớp | Công nghệ | Trách nhiệm |
|---|---|---|
| Lưu trữ Frontend | AWS Amplify | Phục vụ ứng dụng React + Vite SPA; quản lý CDN và HTTPS |
| Xác thực (Auth) | AWS Cognito | Đăng ký người dùng, quản lý token JWT, trigger Lambda sau xác nhận |
| Mạng (Network) | VPC · ALB · NAT Gateway · Internet Gateway · Gateway Endpoints | Cách ly mạng con công cộng/dùng riêng; định tuyến lưu lượng đến EC2; cho phép truy cập S3/DynamoDB an toàn từ mạng con dùng riêng |
| Tính toán (Compute) | AWS EC2 (mạng con dùng riêng) | Chạy máy chủ API FastAPI, worker Celery và broker Redis |
| Gỡ băng | AWS Transcribe + Lambda | Chuyển đổi giọng nói thành văn bản; Lambda (`audio2text_lambda.py`) được kích hoạt khi tạo đối tượng Âm thanh trên S3 |
| Kho lưu trữ Vector | Sentence Transformers + S3 Vectors bucket | Tạo nhúng, phân mảnh, tìm kiếm ngữ nghĩa; các vector được lưu bền vững vào bucket S3 chuyên dụng |
| Lớp LLM | LiteLLM (model-agnostic) | Lớp trừu tượng cho OpenAI, Anthropic, Bedrock; các lệnh gọi ra ngoài qua NAT Gateway |
| Lưu trữ | S3 (Âm thanh) · S3 (Bản chép lời) · S3 (Vector) · DynamoDB | Ba bucket S3 chuyên dụng; DynamoDB cho trạng thái, siêu dữ liệu và bộ nhớ trò chuyện |

#### Các dịch vụ AWS được sử dụng

- **AWS Amplify**: Lưu trữ frontend React + Vite (bước 0 trong biểu đồ). Người dùng truy cập ứng dụng trực tiếp thông qua endpoint CDN được quản lý của Amplify. Token JWT của Cognito được lấy tại đây (bước 1) và được truyền đi cùng tất cả các lệnh gọi API sau đó.
- **Amazon Cognito**: Quản lý đăng ký, đăng nhập và quản lý phiên JWT của người dùng. Một trigger sau khi xác nhận sẽ kích hoạt một hàm Lambda (bước 2) để cung cấp bản ghi người dùng mới trong DynamoDB (bước 3).
- **AWS Lambda (user_creation_db.py)**: Trigger sau xác nhận của Cognito; tạo mục nhập DynamoDB của người dùng trong lần đăng ký đầu tiên (bước 2–3).
- **Application Load Balancer (ALB)**: Nằm trong mạng con công cộng và nhận tất cả lưu lượng API từ Amplify (bước 4). Định tuyến các yêu cầu thông qua Gateway Endpoints vào mạng con dùng riêng (bước 5), kết thúc TLS và phân phối tải tới mục tiêu EC2.
- **VPC Gateway Endpoints**: Được triển khai trong mạng con dùng riêng, chúng cho phép phiên bản EC2 tiếp cận S3 và DynamoDB qua mạng riêng của AWS mà không cần đi qua NAT Gateway (bước 5–6), giúp cải thiện bảo mật và giảm chi phí xử lý dữ liệu NAT.
- **AWS EC2 (mạng con dùng riêng)**: Nút tính toán cốt lõi. Chạy máy chủ API FastAPI, worker Celery, và broker Redis dưới dạng các tiến trình cùng vị trí (co-located processes). Xử lý toàn bộ logic nghiệp vụ: tạo URL presigned, Hỏi & Đáp RAG, vector hóa và quản lý trạng thái. Truy cập DynamoDB (bước 7), S3 Vectors (bước 13) và S3 Transcript (bước 12) qua Gateway Endpoints; tiếp cận các API LLM ngoại vi qua NAT Gateway (bước 14).
- **NAT Gateway + Internet Gateway**: Cung cấp quyền truy cập internet ra bên ngoài từ phiên bản EC2 trong mạng con dùng riêng (bước 14 → 15 → 16), được sử dụng độc quyền cho các lệnh gọi API LLM ngoại vi thông qua LiteLLM. Lưu lượng truy cập vào luôn được định tuyến thông qua ALB.
- **AWS S3 — Bucket Âm thanh (Audio)**: Nhận các tệp âm thanh tải lên qua các URL presigned (bước 8). Một thông báo sự kiện S3 sẽ kích hoạt Lambda gỡ băng khi có đối tượng được tạo (bước 9).
- **AWS Lambda (audio2text_lambda.py)**: Được kích hoạt bởi bucket S3 Audio khi tạo đối tượng (bước 9). Khởi chạy một tác vụ AWS Transcribe (bước 10).
- **AWS Transcribe**: Thực hiện chuyển đổi giọng nói thành văn bản trên tệp âm thanh và ghi bản chép lời đã hoàn thành vào bucket S3 Transcript (bước 11).
- **AWS S3 — Bucket Bản chép lời (Transcript)**: Lưu trữ các bản chép lời đã hoàn thành. Được đọc bởi worker Celery trên EC2 trong quá trình vector hóa (bước 12).
- **AWS S3 — Bucket Vector**: Lưu trữ chỉ mục vector hai cấp độ chi tiết tùy chỉnh (nhúng phân mảnh + tóm tắt phân đoạn + tóm tắt toàn cục). Được đọc và ghi bởi phiên bản EC2 (bước 13).
- **Amazon DynamoDB**: Lưu trữ trạng thái xử lý của từng bản ghi, phần trăm tiến độ, nhãn giai đoạn, tóm tắt ngắn của AI, ID bản chép lời và bộ nhớ trò chuyện của mỗi phiên. Được ghi bởi cả trigger Lambda của người dùng (bước 3) và worker Celery trên EC2 (bước 7).
- **AWS IAM**: Các chính sách dựa trên vai trò giới hạn quyền truy cập của từng dịch vụ — Vai trò Lambda cho S3/DynamoDB/Transcribe; hồ sơ phiên bản EC2 (instance profile) cho S3/DynamoDB; vai trò dịch vụ Amplify cho việc lưu trữ (hosting).

#### Thiết kế thành phần

- **Frontend (AWS Amplify)**: Ứng dụng React + Vite SPA với các trang Landing, Login, Register, Confirm, Dashboard, Library, và trò chuyện Assistant. Trạng thái xác thực được quản lý qua AWS Cognito; các route được bảo vệ sẽ chuyển hướng người dùng chưa xác thực. `AssistantPage` và `LibraryPage` là hai giao diện chính để tương tác với người dùng.
- **Lớp API (EC2 — FastAPI)**: FastAPI với hai router — `recordings` (tạo URL tải lên, trigger xử lý, Hỏi & Đáp, thăm dò trạng thái) và `library` (danh sách và lọc bản ghi). Tất cả các endpoint đều xác thực token JWT của Cognito. Được đặt cùng vị trí trên EC2 cùng với Celery và Redis.
- **Pipeline Bất đồng bộ (EC2 — Celery Worker)**: Một worker Celery (`process_audio_task`) điều phối toàn bộ quy trình: nó thăm dò bucket S3 Audio cho đến khi việc tải lên được xác nhận (`wait_until_uploaded`), đợi tác vụ Transcribe hoàn tất, chạy quá trình vector hóa, và ghi các chuyển đổi trạng thái (`Pending → Processing → Completed / Failed`) vào DynamoDB ở mỗi giai đoạn kèm theo tỷ lệ phần trăm tiến độ và nhãn giai đoạn. Redis (cũng trên EC2) đóng vai trò là broker cho Celery.
- **Chỉ mục Vector & Ngữ nghĩa (EC2 → S3 Vectors)**: Sau khi gỡ băng, bản chép lời được chia thành các phần chồng chéo lên nhau bằng bộ chia văn bản của LangChain, được nhóm thành các phân đoạn chủ đề thông qua thuật toán mạch lạc cửa sổ trượt, và mỗi phân đoạn được tóm tắt bởi LLM. Các vector nhúng (embeddings) được tạo bằng Sentence Transformers (hỗ trợ đa ngôn ngữ) và lưu bền vững vào bucket S3 Vectors ở hai cấp độ: từng đoạn riêng lẻ (cho truy xuất chính xác) và tóm tắt ở cấp độ phân đoạn (cho truy vấn theo chủ đề). Một bản tóm tắt toàn cục được tính toán trước và lưu trữ cho các truy vấn muốn tóm tắt toàn bộ cuộc họp.
- **Bộ định tuyến Câu hỏi RAG**: Mỗi truy vấn của người dùng được phân loại bằng một lệnh gọi LLM (với lược đồ đầu ra có cấu trúc) thành một trong năm ý định (Thực tế, Toàn diện, Tóm tắt Chủ đề, Tóm tắt Toàn bộ, Ngoài chủ đề). Bộ định tuyến sẽ chọn chiến lược truy xuất tương ứng. Các điểm tin cậy nằm dưới một ngưỡng nhất định sẽ tự động kích hoạt nâng cấp lên chiến lược kỹ lưỡng hơn. Các lệnh gọi LLM để phân loại và tạo văn bản sẽ đi ra khỏi VPC thông qua NAT Gateway.
- **Bộ nhớ Trò chuyện (DynamoDB)**: Mỗi phiên giữa người dùng và bản ghi sẽ duy trì một đối tượng bộ nhớ trong DynamoDB, chứa một cửa sổ làm việc cuốn chiếu (10 lượt gần nhất), một bản tóm tắt nén của các lượt cũ hơn, và một nhật ký lịch sử nguồn. Các phiên này vẫn tồn tại sau khi khởi động lại EC2 và có thể được tiếp tục trên nhiều thiết bị.

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

Dự án tuân theo một lộ trình gồm sáu giai đoạn để mang lại một hệ thống sẵn sàng cho môi trường sản xuất trong mười hai tuần:

- **Giai đoạn 1 — Cơ sở hạ tầng & CI/CD (Tuần 1–2)**: Thiết lập môi trường AWS (S3, DynamoDB, Cognito, Transcribe), cấu hình Celery/Redis, pipeline triển khai
- **Giai đoạn 2 — Pipeline Âm thanh Cốt lõi (Tuần 3–4)**: Luồng tải lên với các URL presigned, trigger gỡ băng Lambda, theo dõi trạng thái trong DynamoDB, tích hợp tác vụ Celery
- **Giai đoạn 3 — Động cơ Vector & RAG (Tuần 5–6)**: Phân mảnh văn bản, tạo nhúng Sentence Transformer, xây dựng kho lưu trữ vector, bộ phân loại câu hỏi, các chiến lược truy xuất
- **Giai đoạn 4 — Giao diện API & Trò chuyện (Tuần 7–8)**: Router FastAPI, quản lý bộ nhớ trò chuyện, trang trò chuyện trên frontend, duy trì phiên (session persistence)
- **Giai đoạn 5 — Bảng điều khiển & Thư viện (Tuần 9–10)**: Thư viện bản ghi với các thẻ trạng thái, tìm kiếm/lọc, chế độ xem lịch sử cuộc họp
- **Giai đoạn 6 — QA, Củng cố hệ thống & Ra mắt (Tuần 11–12)**: Kiểm thử tải, xử lý lỗi, tối ưu hóa chi phí, viết tài liệu, phát hành bản sản xuất

#### Yêu cầu kỹ thuật

- **Backend**: Python 3.x, FastAPI 0.135, Celery 5.6, Redis (broker), boto3 1.42, LiteLLM 1.82, Sentence Transformers 5.3, LangChain Text Splitters 1.1, NumPy 2.4, Pydantic 2.12, python-dotenv
- **Frontend**: React + Vite, AWS Amplify/Cognito JS SDK, API client dựa trên Axios
- **Cơ sở hạ tầng**: VPC (ap-southeast-1) với mạng con công cộng (ALB) và mạng con dùng riêng (EC2, Gateway Endpoints); Phiên bản EC2 chạy FastAPI + Celery + Redis; NAT Gateway + Internet Gateway cho quyền truy cập ra ngoài; ba bucket S3 (Âm thanh, Bản chép lời, Vector); DynamoDB (bảng trạng thái bản ghi + bảng người dùng); Cognito User Pool; Lambda (hai hàm); AWS Transcribe; Amplify (lưu trữ frontend); IAM roles
- **LLM**: Lớp trừu tượng LiteLLM; mô hình mục tiêu mặc định là Claude Sonnet; có thể chuyển đổi thông qua cấu hình
- **Định dạng Âm thanh được hỗ trợ**: WAV, MP3, M4A
- **Mẫu Thiết kế chính**: Sử dụng URL tải lên presigned của S3 để vượt qua giới hạn kích thước tệp của API gateway (thường là 10 MB), giảm chi phí dữ liệu đầu ra của backend và cải thiện băng thông tải lên đối với các tệp âm thanh lớn

---

### 5. Lộ trình & Mốc triển khai

#### Lộ trình dự án

| # | Dòng thời gian | Giai đoạn | Sản phẩm bàn giao |
|---|---|---|---|
| 1 | Tuần 1–2 | Cơ sở hạ tầng & CI/CD | Thiết lập AWS, Celery/Redis, pipeline triển khai |
| 2 | Tuần 3–4 | Pipeline Âm thanh Cốt lõi | Luồng tải lên, trigger Lambda, theo dõi trạng thái, tác vụ Celery |
| 3 | Tuần 5–6 | Động cơ Vector & RAG | Phân mảnh, tạo nhúng, kho lưu trữ vector, bộ phân loại câu hỏi, chiến lược truy xuất |
| 4 | Tuần 7–8 | Giao diện API & Trò chuyện | Router FastAPI, quản lý bộ nhớ, trò chuyện frontend, duy trì phiên |
| 5 | Tuần 9–10 | Bảng điều khiển & Thư viện | Thư viện bản ghi, thẻ trạng thái, tìm kiếm/lọc, lịch sử |
| 6 | Tuần 11–12 | QA, Củng cố hệ thống & Ra mắt | Kiểm thử tải, xử lý lỗi, tối ưu hóa chi phí, tài liệu, phát hành bản sản xuất |

**Tổng thời gian ước tính**: 12 tuần

---

### 6. Ước tính ngân sách

#### Chi phí Cơ sở hạ tầng (Dịch vụ AWS — ap-southeast-1, Singapore)

Kiến trúc này bao gồm cả các khoản phí cố định hàng tháng (EC2, ALB, NAT Gateway) và các khoản phí biến đổi (Transcribe, S3, truyền tải dữ liệu). Khác với một thiết kế hoàn toàn serverless, lớp mạng VPC mang theo một mức chi phí cơ sở đáng kể bất kể khối lượng sử dụng.

| Dịch vụ | Chi phí Hàng tháng Ước tính | Yếu tố cấu thành chi phí |
|---|---|---|
| **NAT Gateway** | ~$43/tháng | Cố định $0.059/giờ × 730 giờ + $0.059/GB dữ liệu xử lý; chi phí chủ đạo — định tuyến các lệnh gọi API LLM ngoại vi của EC2 |
| **ALB (Application Load Balancer)** | ~$18/tháng | Cơ sở ~$0.008/giờ + phí LCU; chi phí tối thiểu ở mức khối lượng yêu cầu thấp |
| **AWS EC2 (t3.small, mạng con dùng riêng)** | ~$15/tháng | Đặt cùng vị trí FastAPI + Celery + Redis; t3.small ($0.0208/giờ × 730 giờ); có thể nâng cấp lên t3.medium (~$30/tháng) nếu cần |
| **AWS Amplify** | ~$0.01–0.05/tháng | Lưu trữ frontend; không đáng kể ở mức lưu lượng thấp |
| **AWS S3 (3 bucket)** | ~$0.10–0.30/tháng | Các bucket Âm thanh, Bản chép lời và Vector; tăng theo khối lượng bản ghi |
| **AWS Transcribe** | ~$0.02–0.30/tháng | $0.024/phút; biến động cao — tỷ lệ thuận trực tiếp với số giờ âm thanh được xử lý |
| **AWS Lambda (2 hàm)** | ~$0.00/tháng | Hoàn toàn nằm trong gói miễn phí (1 triệu yêu cầu/tháng) |
| **Amazon DynamoDB** | ~$0.00–0.05/tháng | Định giá theo yêu cầu; khối lượng đọc/ghi thấp ở quy mô ban đầu |
| **Amazon Cognito** | ~$0.00/tháng | Gói miễn phí: 50.000 người dùng hoạt động hàng tháng (MAU) đầu tiên |
| **Truyền dữ liệu (vào/ra)** | ~$0.05–0.15/tháng | S3 chiều vào miễn phí; egress chiều ra từ ALB và EC2 với giá $0.09/GB |

**Tổng ước tính: ~$76–$77/tháng ở quy mô tối thiểu** (chủ yếu do NAT Gateway ~$43 + ALB ~$18 + EC2 ~$15)

> **Lưu ý tối ưu hóa chi phí**: NAT Gateway là yếu tố cấu thành chi phí lớn nhất (chiếm ~56% mức cơ sở cố định). Nếu khối lượng gọi API LLM thấp hoặc có thể gộp nhóm (batch), một VPC Interface Endpoint cho Bedrock (nếu sử dụng các mô hình lưu trữ trên AWS qua LiteLLM) có thể loại bỏ lưu lượng NAT Gateway cho các lệnh gọi LLM và giảm đáng kể chi phí này. Hoặc, việc chuyển sang một phiên bản EC2 trong mạng con công cộng với một Elastic IP sẽ loại bỏ hoàn toàn NAT Gateway, mặc dù sẽ phải đánh đổi một chút về mặt bảo mật.

#### Chi phí phát triển một lần

- Cấu hình tài khoản AWS, thiết lập VPC, tạo IAM role và pipeline CI/CD: tối thiểu, sử dụng AWS CDK hoặc console
- Không yêu cầu phần cứng độc quyền — hoàn toàn trên nền tảng đám mây

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

| Rủi ro | Tác động | Xác suất | Biện pháp giảm nhẹ |
|---|---|---|---|
| Tăng đột biến độ trễ gỡ băng | Cao (pipeline bị đình trệ, UX kém cho tệp lớn) | Trung bình | Pipeline bất đồng bộ theo dõi tiến độ thời gian thực; thời gian chờ có thể cấu hình cùng logic thử lại trong tác vụ Celery (`max_retries=3`, `countdown=60`) |
| Vượt ngân sách API LLM | Trung bình (áp lực ngân sách khi mở rộng) | Thấp | Ngân sách token cho mỗi truy vấn; lưu cache các tóm tắt phân đoạn để giảm lệnh gọi tổng hợp; LiteLLM cho phép chuyển sang các mô hình rẻ hơn qua cấu hình |
| Lỗi tải lên S3 | Trung bình (mất bản ghi trước khi xử lý) | Thấp | Worker thăm dò S3 qua `wait_until_uploaded` trước khi tiếp tục; trạng thái được ghi vào DynamoDB ngay lập tức; trạng thái lỗi hiển thị trên UI |
| Hỏng chỉ mục vector | Cao (Hỏi & Đáp không khả dụng cho bản ghi) | Thấp | Kho lưu trữ vector được định danh theo ID bản ghi; có thể kích hoạt lại việc vector hóa bằng cách chạy lại tác vụ Celery |
| Hết hạn token xác thực giữa phiên | Thấp (người dùng bị đăng xuất đột ngột) | Trung bình | Xác thực JWT trên mỗi lệnh gọi API; Cognito xử lý việc làm mới token; frontend chuyển hướng mượt mà khi gặp lỗi 401 |

#### Chiến lược giảm nhẹ

- **Khả năng phục hồi bất đồng bộ**: Các tác vụ Celery bao gồm việc tự động thử lại với thời gian lùi theo cấp số nhân (exponential back-off) đối với các lỗi tạm thời; các chuyển đổi trạng thái được ghi bền vững vào DynamoDB ở mọi giai đoạn
- **Phòng ngừa ảo giác (Hallucination)**: Dấu nhắc hệ thống LLM cơ sở hướng dẫn mô hình chỉ trả lời dựa trên ngữ cảnh bản chép lời được cung cấp; bộ phân loại ngoài chủ đề của bộ định tuyến câu hỏi tiếp tục ngăn chặn các lệnh gọi LLM không liên quan
- **Quản lý bí mật (Secret management)**: Tất cả các khóa API, tên bucket, tên bảng DynamoDB và khu vực AWS được lưu trữ dưới dạng biến môi trường được tải qua `python-dotenv`; không có thông tin bí mật nào được đưa vào quản lý mã nguồn |

#### Kế hoạch dự phòng

- Nếu AWS Transcribe không khả dụng, tác vụ Celery sẽ thất bại một cách an toàn và đánh dấu bản ghi là `Failed` trong DynamoDB; UI sẽ hiển thị lỗi kèm theo tùy chọn thử lại
- Nếu nhà cung cấp LLM không khả dụng, LiteLLM có thể được cấu hình lại để dự phòng bằng một nhà cung cấp thay thế mà không cần thay đổi mã ứng dụng
- Có thể sử dụng các ngăn xếp (stacks) CloudFormation hoặc CDK để khôi phục nhanh (rollback) cơ sở hạ tầng nếu bản triển khai gây ra lỗi thoái lui

---

### 8. Kết quả kỳ vọng

#### Cải tiến kỹ thuật

- Bất kỳ từ, quyết định, hoặc mục hành động nào từ bất kỳ cuộc họp được ghi âm nào đều có thể được truy xuất trong vòng vài giây thông qua truy vấn bằng ngôn ngữ tự nhiên
- Chỉ mục vector hai cấp độ chi tiết phục vụ toàn bộ phổ truy vấn — từ việc tra cứu thực tế chính xác đến các tóm tắt chủ đề rộng — một cách hiệu quả và với độ trễ thấp
- Bộ định tuyến câu hỏi RAG giúp giảm bớt các lệnh gọi LLM không cần thiết (và chi phí liên quan) cho các truy vấn đơn giản, đồng thời vẫn đảm bảo mức độ bao phủ toàn diện cho các truy vấn phức tạp
- Việc theo dõi trạng thái theo thời gian thực (đang chờ → đang xử lý → hoàn thành) thay thế các tác vụ hàng loạt thiếu minh bạch bằng một pipeline rõ ràng và người dùng có thể nhìn thấy được

#### Giá trị dài hạn

- Kiến trúc mô-đun của nền tảng (API, worker, kho lưu trữ vector, lớp LLM) cho phép từng thành phần được mở rộng hoặc thay thế độc lập khi các yêu cầu phát triển lên
- Lớp trừu tượng không phụ thuộc mô hình của LiteLLM đảm bảo nền tảng có thể áp dụng các backend LLM mới hơn, rẻ hơn, hoặc nhiều tính năng hơn mà không cần thay đổi kiến trúc
- Kho lưu trữ vector và lớp bộ nhớ trò chuyện tạo thành một nền tảng có thể tái sử dụng cho các tính năng hỗ trợ AI trong tương lai (ví dụ: tìm kiếm chéo nhiều bản ghi, phân tách người nói - speaker diarisation, trích xuất mục hành động)
- Bản chép lời và siêu dữ liệu được lưu trữ trong DynamoDB và S3 cung cấp một cơ sở tri thức bền vững, có thể truy vấn, và sẽ tích lũy giá trị theo thời gian khi thư viện bản ghi ngày càng mở rộng