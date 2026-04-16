---
title: "Đề xuất" 
date: "2026-03-02" 
weight: 2 
chapter: false 
pre: " <b> 2. </b> "
---

# Voice Summarizer — The Archivist
## Nền tảng Trí tuệ Âm thanh được hỗ trợ bởi AI

**Thành viên nhóm:**

| Họ và tên             | Mã sinh viên | Vai trò      |
| --------------------- | ------------ | ------------ |
| Phan Minh Nhật Trường | SE196744     | Trưởng nhóm  |
| Nguyễn Minh Quân      | SE193488     | Thành viên   |
| Phạm Đăng Khoa        | SE192211     | Thành viên   |
| Nguyễn Bá Tùng Dương  | SE193510     | Thành viên   |
| Trịnh Thanh An        | SE193956     | Thành viên   |

**Các dịch vụ AWS:** `Amplify` · `Cognito` · `Lambda` · `S3` · `DynamoDB` · `Transcribe` · `EC2`

---

### 1. Tóm tắt Tổng quan

Voice Summarizer là một nền tảng AI toàn diện giúp chuyển đổi các bản ghi âm — các cuộc họp, bài giảng, phỏng vấn và cuộc gọi — thành các cơ sở kiến thức có thể tìm kiếm và truy vấn được. Người dùng tải lên một tệp âm thanh (WAV, MP3, M4A); hệ thống sẽ tự động phiên âm, lập chỉ mục theo ngữ nghĩa và cung cấp một giao diện AI đàm thoại, qua đó người dùng có thể đặt câu hỏi bằng ngôn ngữ tự nhiên và nhận được những câu trả lời chính xác, bám sát ngữ cảnh.

Nền tảng này được xây dựng trên kiến trúc lai của AWS, triển khai tại khu vực Singapore (ap-southeast-1). Frontend được lưu trữ trên AWS Amplify; backend (FastAPI + Celery + Redis) chạy trên một EC2 instance trong mạng con riêng (private subnet), được đứng trước bởi một Application Load Balancer (ALB) trong mạng con công khai (public subnet). Một NAT Gateway cung cấp cho EC2 instance quyền truy cập internet ra bên ngoài để gọi các LLM API. AWS Transcribe xử lý việc chuyển đổi giọng nói thành văn bản, được kích hoạt bởi một hàm Lambda khi có đối tượng (tệp) S3 mới được tạo; một Lambda thứ hai sẽ cấp phát các bản ghi người dùng trên DynamoDB khi họ đăng ký qua Cognito. Ba bucket S3 chuyên dụng lưu trữ âm thanh gốc, bản phiên âm và dữ liệu vector. Các VPC Gateway Endpoints kết nối mạng con riêng với S3 và DynamoDB mà không cần đi qua internet công cộng. Các lời gọi LLM được định tuyến qua LiteLLM để mang lại sự linh hoạt không phụ thuộc vào mô hình (model-agnostic), và quyền truy cập được bảo mật thông qua Amazon Cognito với xác thực JWT.

---

### 2. Đặt vấn đề

#### Vấn đề là gì?

Mỗi năm, các tổ chức thực hiện hàng ngàn giờ ghi âm các cuộc họp, cuộc gọi với khách hàng, các buổi đào tạo và phỏng vấn. Bất chấp khối lượng này, thông tin bên trong những bản ghi âm đó trên thực tế lại là "dữ liệu chìm" (dark data) — rất khó để tìm kiếm, tóm tắt hoặc sử dụng mà không phải nghe lại từ đầu đến cuối. Những điểm nghẽn hiện tại bao gồm:

  - Không có cách nào nhanh chóng để tìm thấy một quyết định, con số, hoặc tên cụ thể được đề cập trong một bản ghi âm dài.
  - Việc ghi chép thủ công không nhất quán, không đầy đủ và tốn nhiều thời gian.
  - Việc chia sẻ thông tin chi tiết từ các bản ghi âm đòi hỏi phải nghe lại và tóm tắt thủ công.
  - Các trường hợp sử dụng tuân thủ và kiểm toán yêu cầu bằng chứng có thể truy xuất được từ các phiên đã ghi âm.

#### Giải pháp

Voice Summarizer giải quyết những vấn đề này bằng cách tự động hóa toàn bộ quy trình, từ âm thanh gốc đến một giao diện kiến thức tương tác do AI hỗ trợ. Nền tảng cung cấp ba khả năng cốt lõi:

1.  **Quy trình Tự động chuyển Âm thanh thành Kiến thức** — người dùng tải lên tệp âm thanh thông qua URL S3 được ký trước (presigned URL); một Celery worker bất đồng bộ sẽ phiên âm nó thông qua AWS Transcribe (được kích hoạt bởi một hàm Lambda), sau đó chia nhỏ, nhúng (embed), và lập chỉ mục bản phiên âm vào một kho lưu trữ vector ngữ nghĩa — tất cả đều không cần sự can thiệp của người dùng.
2.  **Trợ lý AI Đàm thoại** — mỗi bản ghi âm sẽ có một giao diện trò chuyện riêng biệt, được hỗ trợ bởi một bộ định tuyến câu hỏi RAG. Bộ định tuyến này phân loại ý định của truy vấn và chọn chiến lược truy xuất tối ưu (thực tế, toàn diện, tóm tắt chủ đề, tóm tắt toàn bộ hoặc ngoài lề), tạo ra các câu trả lời chống ảo giác (hallucination-resistant) và có cơ sở từ bản phiên âm.
3.  **Thư viện & Bảng điều khiển Bản ghi âm** — một thư viện có thể tìm kiếm liệt kê tất cả các bản ghi âm đã được xử lý với các chỉ báo trạng thái, bản tóm tắt ngắn do AI tạo, thời lượng và ngày tạo.

#### Lợi ích và Tỷ suất Hoàn vốn (ROI)

Voice Summarizer loại bỏ gánh nặng thủ công của việc phát lại, ghi chép và tóm tắt các bản ghi âm. Các lợi ích chính bao gồm:

  - **Tăng năng suất ngay lập tức** — bất kỳ sự kiện, quyết định hoặc mục hành động nào từ bất kỳ bản ghi âm nào đều có thể được truy xuất trong vài giây.
  - **Giảm rủi ro tuân thủ** — các bản phiên âm đầy đủ và dấu vết kiểm toán được lưu trữ và có thể truy vấn.
  - **Lưu giữ kiến thức** — kiến thức nội bộ của tổ chức được ghi lại trong các bản ghi âm không còn bị mất đi sau khi cuộc họp kết thúc.
  - **Chi phí cơ sở hạ tầng có thể dự đoán** — một EC2 instance với kích thước cố định cung cấp dung lượng tính toán nhất quán, với các chi phí biến đổi được giới hạn ở mức sử dụng Transcribe và truyền tải dữ liệu.

-----

### 3. Kiến trúc Giải pháp

![Proposed Architecture](/images/2-Proposal/AWSWorkshopArchitecture-Final.jpg)

Hệ thống được triển khai tại AWS ap-southeast-1 (Singapore) và bao gồm chín lớp được liên kết lỏng lẻo (loosely coupled):

| Lớp | Công nghệ | Trách nhiệm |
|---|---|---|
| DNS | Amazon Route 53 | Phân giải tên miền tùy chỉnh; định tuyến người dùng đến frontend Amplify và tên miền phụ API đến ALB |
| Lưu trữ Frontend | AWS Amplify | Cung cấp ứng dụng React + Vite SPA; quản lý CDN và HTTPS |
| Xác thực | AWS Cognito | Đăng ký người dùng và quản lý token JWT; các bản ghi người dùng được EC2 cấp phát trong yêu cầu đầu tiên |
| Mạng | VPC · ALB · NAT Gateway · Internet Gateway · Gateway Endpoints | Cách ly mạng con riêng/công khai; định tuyến lưu lượng đến EC2; cho phép truy cập S3/DynamoDB an toàn từ mạng con riêng |
| Tính toán | AWS EC2 (mạng con riêng) | Chạy máy chủ API FastAPI, Celery worker và Redis broker |
| Phiên âm | AWS Transcribe + Lambda | Chuyển đổi giọng nói thành văn bản; một Lambda duy nhất (`audio2text_lambda`) được kích hoạt bởi việc tạo đối tượng S3 `raw_audio/` |
| Lưu trữ Vector | Sentence Transformers + S3 (tiền tố `vectors/`) | Nhúng, chia nhỏ văn bản, tìm kiếm ngữ nghĩa; các vector được lưu trữ dưới một tiền tố chuyên dụng trong bucket S3 dùng chung |
| Lớp LLM | LiteLLM (model-agnostic) | Lớp trừu tượng cho OpenAI, Anthropic, Bedrock; các lời gọi ra bên ngoài đi qua NAT Gateway |
| Lưu trữ | S3 (một bucket, nhiều tiền tố) · DynamoDB (một bảng) | Một bucket S3 tổ chức dữ liệu qua các tiền tố `raw_audio/`, `transcripts/`, `vectors/`, và `summarize/`; một bảng DynamoDB lưu trữ toàn bộ trạng thái của nền tảng |

#### Các Dịch vụ AWS Được Sử Dụng

  - **Amazon Route 53**: Cung cấp DNS cho tên miền tùy chỉnh của nền tảng. Một Public Hosted Zone lưu giữ các bản ghi NS để phân giải tên miền; một bản ghi bí danh A (alias A record) trỏ tên miền frontend đến endpoint CDN của Amplify và một bản ghi bí danh A trỏ tên miền phụ API đến tên DNS của ALB.
  - **AWS Amplify**: Lưu trữ frontend React + Vite. Người dùng truy cập ứng dụng thông qua tên miền được Route 53 phân giải. Các token JWT của Cognito được lấy và gửi kèm theo tất cả các lời gọi API tiếp theo.
  - **Amazon Cognito**: Quản lý việc đăng ký, đăng nhập của người dùng và quản lý phiên JWT. Không có trigger Lambda hậu xác nhận (post-confirmation) — các bản ghi người dùng trong DynamoDB được backend EC2 ghi trực tiếp trong yêu cầu API được xác thực đầu tiên của người dùng.
  - **Application Load Balancer (ALB)**: Nằm trong mạng con công khai và nhận tất cả lưu lượng API từ Amplify. Định tuyến các yêu cầu thông qua Gateway Endpoints vào mạng con riêng, kết thúc TLS và phân phối tải cho mục tiêu EC2.
  - **VPC Gateway Endpoints**: Được triển khai trong mạng con riêng, chúng cho phép EC2 instance tiếp cận S3 và DynamoDB qua mạng riêng của AWS mà không cần đi qua NAT Gateway, giúp tăng cường bảo mật và giảm chi phí xử lý dữ liệu NAT.
  - **AWS EC2 (mạng con riêng)**: Nút tính toán cốt lõi. Chạy máy chủ API FastAPI, Celery worker và Redis broker dưới dạng các tiến trình cùng vị trí. Ghi các bản ghi người dùng vào DynamoDB trong yêu cầu đầu tiên, cập nhật trạng thái bản ghi âm, đọc bản phiên âm từ S3, ghi bản tóm tắt và vector, đồng thời cập nhật bộ nhớ hội thoại AI. Tiếp cận các LLM API bên ngoài thông qua NAT Gateway.
  - **NAT Gateway + Internet Gateway**: Cung cấp quyền truy cập internet ra bên ngoài cho EC2 instance trong mạng con riêng, được sử dụng độc quyền cho các lời gọi LLM API bên ngoài thông qua LiteLLM. Lưu lượng truy cập đến luôn đi qua ALB.
  - **AWS S3 (một bucket duy nhất, `one4allthing`)**: Một bucket phục vụ mọi nhu cầu lưu trữ, được tổ chức thành bốn tiền tố — `raw_audio/` nhận các tệp tải lên qua URL được ký trước, `transcripts/` nhận đầu ra từ Transcribe, `summarize/` lưu trữ các bản tóm tắt cuộc họp tổng thể được tính toán trước, và `vectors/` lưu trữ chỉ mục ngữ nghĩa hai mức độ chi tiết. Thông báo sự kiện S3 trên `raw_audio/` sẽ kích hoạt Lambda phiên âm.
  - **AWS Lambda (audio2text\_lambda.py)**: Hàm Lambda duy nhất. Được kích hoạt bởi tiền tố `raw_audio/` trên S3 khi tạo đối tượng. Khởi chạy một công việc AWS Transcribe.
  - **AWS Transcribe**: Thực hiện chuyển đổi giọng nói thành văn bản trên tệp âm thanh và ghi bản phiên âm đã hoàn tất vào tiền tố `transcripts/` của bucket S3.
  - **Amazon DynamoDB (một bảng duy nhất)**: Một bảng lưu trữ tất cả các trạng thái của nền tảng — trạng thái và tiến độ của luồng ghi âm, bộ nhớ hội thoại AI và lịch sử trò chuyện theo từng phiên, cùng với các bản ghi người dùng do EC2 ghi vào ở lần đăng nhập đầu tiên.
  - **AWS IAM**: Các chính sách (policies) dựa trên vai trò sẽ giới hạn phạm vi truy cập của từng dịch vụ — Role của Lambda cho S3/Transcribe; Instance profile của EC2 cho S3/DynamoDB; Service role của Amplify cho việc lưu trữ.

#### Thiết kế Thành phần

  - **DNS (Route 53)**: Một Public Hosted Zone cho tên miền tùy chỉnh lưu giữ các bản ghi NS. Hai bản ghi bí danh A định tuyến tên miền gốc (apex) hoặc miền phụ `www` tới endpoint CDN của Amplify và tên miền phụ `api.` tới tên DNS của ALB, loại bỏ việc người dùng phải tham chiếu tới các URL gốc của AWS.
  - **Frontend (AWS Amplify)**: Ứng dụng React + Vite SPA với các trang: Hạ cánh (Landing), Đăng nhập, Đăng ký, Xác nhận, Bảng điều khiển, Thư viện và trò chuyện Trợ lý AI. Trạng thái xác thực được quản lý qua AWS Cognito; các route được bảo vệ sẽ chuyển hướng người dùng chưa xác thực. `AssistantPage` và `LibraryPage` là các giao diện chính dành cho người dùng.
  - **Lớp API (EC2 — FastAPI)**: FastAPI với hai bộ định tuyến (routers) — `recordings` (tạo URL tải lên, kích hoạt xử lý, Hỏi & Đáp, thăm dò trạng thái) và `library` (liệt kê và lọc các bản ghi âm). Tất cả các endpoint đều xác thực token JWT của Cognito. Trong yêu cầu được xác thực đầu tiên của người dùng, lớp FastAPI ghi bản ghi người dùng của họ trực tiếp vào DynamoDB, thay thế sự cần thiết của trigger Lambda từ Cognito. Nằm cùng vị trí trên EC2 với Celery và Redis.
  - **Luồng Bất đồng bộ (EC2 — Celery Worker)**: Một Celery worker (`process_audio_task`) điều phối toàn bộ quy trình: thăm dò tiền tố `raw_audio/` cho đến khi xác nhận đã tải lên xong (`wait_until_uploaded`), chờ công việc Transcribe hoàn tất, chạy quá trình vector hóa, và ghi các chuyển đổi trạng thái (`pending → processing → completed / failed`) vào DynamoDB ở mỗi giai đoạn. Redis (cũng trên EC2) đóng vai trò là broker của Celery.
  - **Chỉ mục Vector & Ngữ nghĩa (EC2 → S3 `vectors/` + `summarize/`)**: Sau khi phiên âm, bản phiên âm được chia thành các đoạn chồng lấn lên nhau bằng bộ chia văn bản (text splitter) của LangChain, được nhóm thành các phân đoạn theo chủ đề và mỗi phân đoạn được tóm tắt bởi LLM. Các vector nhúng (embeddings) được tạo bằng Sentence Transformers và lưu trữ dưới tiền tố `vectors/`. Các bản tóm tắt tổng thể tính toán trước được lưu trữ dưới `summarize/` cho các phản hồi tóm tắt toàn bộ cuộc họp tức thì — tất cả nằm trong cùng một bucket S3.
  - **Bộ định tuyến câu hỏi RAG**: Mỗi truy vấn của người dùng được một lời gọi LLM phân loại vào một trong năm ý định (Thực tế, Toàn diện, Tóm tắt Chủ đề, Tóm tắt Toàn bộ, Ngoài lề). Bộ định tuyến chọn chiến lược truy xuất phù hợp. Lời gọi LLM thoát khỏi VPC thông qua NAT Gateway.
  - **Bộ nhớ Hội thoại (DynamoDB)**: Mỗi phiên tương tác ghi âm của người dùng sẽ duy trì một đối tượng bộ nhớ trong DynamoDB chứa một cửa sổ làm việc cuốn chiếu (10 lượt gần nhất), một bản tóm tắt nén của các lượt cũ hơn và nhật ký lịch sử nguồn. Các phiên này tồn tại ngay cả khi EC2 khởi động lại và có thể được tiếp tục trên các thiết bị khác nhau.

-----

### 4\. Triển khai Kỹ thuật

#### Các giai đoạn triển khai

Dự án tuân theo lộ trình sáu giai đoạn để cung cấp một hệ thống sẵn sàng cho sản xuất trong mười hai tuần:

  - **Giai đoạn 1 — Cơ sở hạ tầng & CI/CD (Tuần 1–2)**: Thiết lập môi trường AWS (S3, DynamoDB, Cognito, Transcribe), cấu hình Celery/Redis, luồng triển khai (deployment pipeline).
  - **Giai đoạn 2 — Luồng Âm thanh Cốt lõi (Tuần 3–4)**: Quy trình tải lên với các URL được ký trước, kích hoạt phiên âm Lambda, theo dõi trạng thái trong DynamoDB, tích hợp tác vụ Celery.
  - **Giai đoạn 3 — Engine Vector & RAG (Tuần 5–6)**: Chia nhỏ văn bản, nhúng bằng Sentence Transformer, xây dựng kho lưu trữ vector, bộ phân loại câu hỏi, các chiến lược truy xuất.
  - **Giai đoạn 4 — API & Giao diện Trò chuyện (Tuần 7–8)**: Các bộ định tuyến FastAPI, quản lý bộ nhớ hội thoại, trang trò chuyện frontend, tính bền vững của phiên (session persistence).
  - **Giai đoạn 5 — Bảng điều khiển & Thư viện (Tuần 9–10)**: Thư viện bản ghi âm với các thẻ trạng thái, tìm kiếm/lọc, chế độ xem lịch sử cuộc họp.
  - **Giai đoạn 6 — QA, Tăng cường Bảo mật & Ra mắt (Tuần 11–12)**: Kiểm thử chịu tải, xử lý lỗi, tối ưu hóa chi phí, viết tài liệu, phát hành bản sản xuất.

#### Yêu cầu Kỹ thuật

  - **Backend**: Python 3.x, FastAPI 0.135, Celery 5.6, Redis (broker), boto3 1.42, LiteLLM 1.82, Sentence Transformers 5.3, LangChain Text Splitters 1.1, NumPy 2.4, Pydantic 2.12, python-dotenv
  - **Frontend**: React + Vite, SDK AWS Amplify/Cognito JS, API client dựa trên Axios
  - **Cơ sở hạ tầng**: VPC (ap-southeast-1) với mạng con công khai (ALB) và mạng con riêng (EC2, Gateway Endpoints); EC2 instance chạy FastAPI + Celery + Redis; NAT Gateway + Internet Gateway cho truy cập ra ngoài; ba bucket S3 (Âm thanh, Bản phiên âm, Vector); DynamoDB (trạng thái bản ghi âm + bảng người dùng); Cognito User Pool; Lambda (hai hàm); AWS Transcribe; Amplify (lưu trữ frontend); IAM roles
  - **LLM**: Lớp trừu tượng LiteLLM; mô hình mục tiêu mặc định là Claude Sonnet; có thể chuyển đổi thông qua cấu hình
  - **Định dạng âm thanh được hỗ trợ**: WAV, MP3, M4A
  - **Mẫu Thiết kế Chính**: Các URL S3 tải lên được ký trước giúp vượt qua giới hạn kích thước tệp của API gateway (thường là 10 MB), giảm chi phí dữ liệu đi ra từ backend và cải thiện thông lượng tải lên đối với các tệp âm thanh lớn.

-----

### 5\. Dòng thời gian & Cột mốc

#### Tiến độ Dự án

| STT | Thời gian | Giai đoạn | Sản phẩm bàn giao |
|---|---|---|---|
| 1 | Tuần 1–2 | Cơ sở hạ tầng & CI/CD | Thiết lập AWS, Celery/Redis, luồng triển khai |
| 2 | Tuần 3–4 | Luồng Âm thanh Cốt lõi | Luồng tải lên, kích hoạt Lambda, theo dõi trạng thái, tác vụ Celery |
| 3 | Tuần 5–6 | Engine Vector & RAG | Chia nhỏ, nhúng, kho lưu trữ vector, bộ phân loại câu hỏi, chiến lược truy xuất |
| 4 | Tuần 7–8 | API & Giao diện Trò chuyện | Bộ định tuyến FastAPI, quản lý bộ nhớ, trò chuyện frontend, lưu phiên |
| 5 | Tuần 9–10 | Bảng điều khiển & Thư viện | Thư viện bản ghi âm, thẻ trạng thái, tìm kiếm/lọc, lịch sử |
| 6 | Tuần 11–12 | QA, Tăng cường Bảo mật & Ra mắt | Kiểm thử chịu tải, xử lý lỗi, tối ưu chi phí, tài liệu, phát hành |

**Tổng thời gian dự kiến**: 12 tuần

-----

### 6\. Ước tính Ngân sách

#### Chi phí Cơ sở hạ tầng (Các Dịch vụ AWS — ap-southeast-1, Singapore)

Kiến trúc này bao gồm cả chi phí cố định hàng tháng (EC2, ALB, NAT Gateway) và chi phí biến đổi (Transcribe, S3, truyền tải dữ liệu). Khác với thiết kế hoàn toàn không máy chủ (serverless), lớp mạng VPC mang theo một mức chi phí cơ bản đáng kể bất kể dung lượng sử dụng.

| Dịch vụ | Chi phí Hàng tháng Ước tính | Yếu tố Chi phí |
|---|---|---|
| **NAT Gateway** | \~$43/tháng | $0.059/giờ × 730 giờ cố định + $0.059/GB dữ liệu được xử lý; chi phí chiếm ưu thế — định tuyến các lời gọi LLM API ra ngoài từ EC2 |
| **ALB (Application Load Balancer)** | ~$18/tháng | Phí cơ sở \~$0.008/giờ + phí LCU; chi phí tối thiểu ở mức khối lượng yêu cầu thấp |
| **AWS EC2 (t3.small, mạng con riêng)** | ~$15/tháng | FastAPI + Celery + Redis chạy cùng nhau; t3.small ($0.0208/giờ × 730 giờ); có thể nâng cấp lên t3.medium (~$30/tháng) nếu cần |
| **Amazon Route 53** | \~$0.50/tháng | $0.50/tháng cho mỗi Public Hosted Zone; cộng thêm $0.40/triệu truy vấn DNS (không đáng kể ở lưu lượng thấp) |
| **AWS Amplify** | ~$0.01–0.05/tháng | Lưu trữ Frontend; không đáng kể ở lưu lượng thấp |
| **AWS S3 (1 bucket, 4 tiền tố)** | \~$0.05–0.20/tháng | Bucket duy nhất `one4allthing` với các tiền tố `raw_audio/`, `transcripts/`, `vectors/`, `summarize/`; tăng tỷ lệ thuận với khối lượng bản ghi âm |
| **AWS Transcribe** | ~$0.02–0.30/tháng | $0.024/phút; dao động mạnh — tỷ lệ thuận trực tiếp với số giờ âm thanh được xử lý |
| **AWS Lambda (2 hàm)** | ~$0.00/tháng | Hoàn toàn nằm trong bậc miễn phí (1 triệu yêu cầu/tháng) |
| **Amazon DynamoDB (1 bảng)** | \~$0.00–0.05/tháng | Định giá theo nhu cầu (On-demand); một bảng duy nhất lưu trữ toàn bộ trạng thái nền tảng; khối lượng đọc/ghi thấp ở quy mô ban đầu |
| **Amazon Cognito** | ~$0.00/tháng | Bậc miễn phí: 50.000 người dùng hoạt động hàng tháng (MAUs) đầu tiên |
| **Truyền tải Dữ liệu (vào/ra)** | \~$0.05–0.15/tháng | Dữ liệu vào S3 miễn phí; dữ liệu ra của ALB và EC2 ở mức $0.09/GB |

**Tổng chi phí ước tính: \~$76–$77/tháng ở quy mô tối thiểu** (chiếm phần lớn là NAT Gateway \~$43 + ALB ~$18 + EC2 \~$15)

> **Lưu ý tối ưu hóa chi phí**: NAT Gateway là yếu tố tạo ra chi phí lớn nhất (\~56% mức cơ sở cố định). Nếu khối lượng lời gọi LLM API thấp hoặc có thể gom nhóm (batch), việc sử dụng VPC Interface Endpoint cho Bedrock (nếu dùng các mô hình do AWS lưu trữ qua LiteLLM) có thể loại bỏ lưu lượng NAT Gateway cho các lời gọi LLM và làm giảm đáng kể chi phí này. Ngoài ra, việc chuyển sang một EC2 instance ở mạng con công khai với một Elastic IP sẽ loại bỏ hoàn toàn NAT Gateway, mặc dù sẽ phải đánh đổi một chút về mặt bảo mật.

#### Chi phí Phát triển Một lần

  - Cấu hình tài khoản AWS, thiết lập VPC, tạo IAM role và luồng CI/CD: tối thiểu, sử dụng AWS CDK hoặc console
  - Không yêu cầu phần cứng độc quyền — hoàn toàn dựa trên điện toán đám mây (cloud-native)

-----

### 7\. Đánh giá Rủi ro

#### Ma trận Rủi ro

| Rủi ro | Tác động | Xác suất | Giảm thiểu |
|---|---|---|---|
| Độ trễ phiên âm tăng đột biến | Cao (luồng xử lý bị đình trệ, UX kém cho các tệp lớn) | Trung bình | Luồng bất đồng bộ với theo dõi tiến độ theo thời gian thực; thời gian chờ có thể cấu hình kèm logic thử lại trong tác vụ Celery (`max_retries=3`, `countdown=60`) |
| Vượt chi phí LLM API | Trung bình (áp lực ngân sách ở quy mô lớn) | Thấp | Ngân sách token cho mỗi truy vấn; các bản tóm tắt phân đoạn được lưu cache giúp giảm các lời gọi tổng hợp; LiteLLM cho phép chuyển sang các mô hình rẻ hơn qua cấu hình |
| Lỗi tải lên S3 | Trung bình (mất bản ghi âm trước khi xử lý) | Thấp | Worker thăm dò S3 thông qua `wait_until_uploaded` trước khi tiếp tục; trạng thái được ghi vào DynamoDB ngay lập tức; trạng thái lỗi hiển thị trên UI |
| Hỏng chỉ mục vector | Cao (Hỏi & Đáp không khả dụng cho bản ghi âm) | Thấp | Kho lưu trữ vector được khóa bằng ID bản ghi âm; việc vector hóa lại có thể được kích hoạt bằng cách chạy lại tác vụ Celery |
| Token xác thực hết hạn giữa phiên | Thấp (người dùng bị đăng xuất ngoài ý muốn) | Trung bình | Xác thực JWT trên mọi lời gọi API; Cognito xử lý việc làm mới token; frontend chuyển hướng mượt mà khi gặp lỗi 401 |

#### Chiến lược Giảm thiểu

  - **Khả năng phục hồi bất đồng bộ**: Các tác vụ Celery bao gồm việc tự động thử lại với độ trễ tăng dần (exponential back-off) cho các lỗi tạm thời; các chuyển đổi trạng thái được ghi bền vững vào DynamoDB ở mỗi giai đoạn.
  - **Ngăn chặn ảo giác**: Lời nhắc hệ thống (system prompt) LLM cơ bản chỉ thị mô hình chỉ trả lời từ ngữ cảnh bản phiên âm được cung cấp; bộ phân loại ngoài lề của bộ định tuyến câu hỏi tiếp tục ngăn chặn các lời gọi LLM không liên quan.
  - **Quản lý khóa bí mật**: Tất cả các khóa API, tên bucket, tên bảng DynamoDB và khu vực AWS được lưu trữ dưới dạng các biến môi trường và nạp qua `python-dotenv`; không có khóa bí mật nào được commit vào mã nguồn.

#### Kế hoạch Dự phòng

  - Nếu AWS Transcribe không khả dụng, tác vụ Celery sẽ dừng lỗi một cách có kiểm soát và đánh dấu bản ghi âm là `failed` (thất bại) trong DynamoDB; UI sẽ hiển thị thông báo lỗi cùng với tùy chọn thử lại.
  - Nếu nhà cung cấp LLM không khả dụng, LiteLLM có thể được cấu hình lại để chuyển sang nhà cung cấp thay thế mà không cần thay đổi mã nguồn ứng dụng.
  - CloudFormation hoặc các ngăn xếp CDK có thể được sử dụng để khôi phục nhanh cơ sở hạ tầng nếu một bản triển khai gây ra lỗi hồi quy (regressions).

-----

### 8. Kết quả Mong đợi

#### Cải tiến Kỹ thuật

  - Bất kỳ từ ngữ, quyết định hoặc mục hành động nào từ bất kỳ cuộc họp đã ghi âm nào đều có thể được truy xuất trong vài giây thông qua truy vấn ngôn ngữ tự nhiên.
  - Chỉ mục vector hai mức độ chi tiết phục vụ toàn bộ phổ truy vấn — từ tra cứu thông tin thực tế chính xác đến tóm tắt chủ đề tổng quát — một cách hiệu quả và với độ trễ thấp.
  - Bộ định tuyến câu hỏi RAG giúp giảm bớt các lời gọi LLM không cần thiết (và các chi phí liên quan) đối với các truy vấn đơn giản trong khi vẫn đảm bảo độ bao phủ toàn diện cho các truy vấn phức tạp.
  - Theo dõi trạng thái theo thời gian thực (đang chờ → đang xử lý → hoàn tất) thay thế các công việc hàng loạt không rõ ràng bằng một quy trình minh bạch, người dùng có thể nhìn thấy được.

#### Giá trị Dài hạn

  - Kiến trúc mô-đun của nền tảng (API, worker, kho lưu trữ vector, lớp LLM) cho phép từng thành phần được mở rộng quy mô hoặc thay thế một cách độc lập khi yêu cầu thay đổi.
  - Lớp trừu tượng model-agnostic của LiteLLM đảm bảo nền tảng có thể áp dụng các backend LLM mới hơn, rẻ hơn hoặc có khả năng tốt hơn mà không cần thay đổi kiến trúc.
  - Kho lưu trữ vector và lớp bộ nhớ hội thoại hình thành nền tảng có thể tái sử dụng cho các tính năng AI trong tương lai (ví dụ: tìm kiếm chéo giữa các bản ghi âm, phân biệt giọng người nói, trích xuất mục hành động).
  - Bản phiên âm và siêu dữ liệu (metadata) được lưu trữ trong DynamoDB và S3 cung cấp một cơ sở kiến thức bền vững, có thể truy vấn, liên tục tích lũy giá trị theo thời gian khi thư viện bản ghi âm ngày càng lớn mạnh.