---
title: "Sự kiện 3"
date: "2025-11-15"
weight: 03
chapter: false
pre: " <b> 4.3. </b> "
---

# Báo Cáo Tóm Tắt: “AWS Cloud Mastery Series #1 - AI/ML/GenAI trên AWS”

### Mục tiêu Sự kiện

- Giới thiệu về **AI/ML/GenAI** trên **AWS**

### Diễn giả

- **Lam Tuan Kiet** – Sr DevOps Engineer, FPT Software
- **Danh Hoang Hieu Nghi** - AI Engineer, Renova Cloud
- **Dinh Le Hoang Anh** - Cloud Engineer Trainee, First Cloud AI Journey
- **Van Hoang Kha** - Cloud Security Engineer, AWS Community Builder
### Điểm nhấn chính

# Khám phá Generative AI với Amazon Bedrock:

**- Foundation Models (Mô hình Nền tảng):** Khác với Traditional Model (Mô hình Truyền thống) ở chỗ có thể được điều chỉnh cho nhiều tác vụ, cung cấp nhiều mô hình được quản lý hoàn toàn (**fully managed model**) từ các công ty AI hàng đầu như: OpenAI, Claude, Anthropic, v.v.
**- Prompt Engineering:** Soạn thảo và Tinh chỉnh Lời nhắc (Instructions)
   + **Zero-Shot Prompting:** Một lời nhắc không có ngữ cảnh hoặc ví dụ trước
   + **Few-shot Prompting:** Một lời nhắc với một vài ngữ cảnh và ví dụ trước
   + **Chain of Thought (Chuỗi Suy nghĩ):** Một lời nhắc bao gồm các quá trình suy nghĩ và các bước trước khi đưa ra câu trả lời thực tế
**- Retrieval Augmented Generation (RAG):** Truy xuất thông tin liên quan từ một nguồn dữ liệu
   + **R: Retrieval (Truy xuất) -** Lấy thông tin liên quan từ một kho kiến thức hoặc nguồn dữ liệu
   + **A: Augmented (Tăng cường) -** Thêm thông tin được truy xuất làm ngữ cảnh bổ sung vào lời nhắc của người dùng trước khi đưa vào mô hình
   + **G: Generation (Tạo ra) -** Phản hồi từ mô hình cho lời nhắc đã được tăng cường
   + **Các trường hợp sử dụng:** Cải thiện chất lượng nội dung, chatbot và trả lời câu hỏi theo ngữ cảnh, tìm kiếm được cá nhân hóa và tóm tắt dữ liệu thời gian thực

**- Amazon Titan Embedding:** Mô hình nhẹ, nổi bật trong việc dịch văn bản thành biểu diễn số (**embeddings**) cho các tác vụ truy xuất độ chính xác cao, với sự hỗ trợ cho hơn 100 ngôn ngữ

**- Pretrained AI Services (Các Dịch vụ AI đã được đào tạo trước):**
  + **Amazon Rekognition:** Phân tích Hình ảnh và Video
  + **Amazon Translate:** Phát hiện và dịch văn bản
  + **Amazon Textract:** Trích xuất Văn bản và Bố cục từ tài liệu
  + **Amazon Transcribe:** Chuyển lời nói thành văn bản (**Speech-to-text**)
  + **Amazon Polly:** Chuyển văn bản thành lời nói (**Text-to-speech**)
  + **Amazon Comprehend:** Trích xuất Thông tin chi tiết và Mối quan hệ từ văn bản
  + **Amazon Kendra:** Dịch vụ Tìm kiếm Thông minh
  + **Amazon Lookout:** Phát hiện Bất thường (**Anomalies**) trong các chỉ số kinh doanh, thiết bị và hình ảnh
  + **Amazon Personalize:** Điều chỉnh các đề xuất cho người dùng

**- Demo:** AMZPhoto: Nhận diện khuôn mặt từ hình ảnh bằng AI

**- Amazon Bedrock AgentCore:** Một nền tảng đại lý (**agentic platform**) toàn diện được thiết kế để giải quyết các thách thức trong việc đưa **agents** vào sản xuất:
  + Thực thi và mở rộng mã **agent** một cách an toàn.
  + Tích hợp bộ nhớ (**memory**) (ghi nhớ các tương tác trong quá khứ và học hỏi).
  + Triển khai kiểm soát danh tính và truy cập (**identity and access controls**) cho **agents** và các công cụ (**tools**).
  + Cung cấp việc sử dụng công cụ đại lý (**agentic tool use**) cho các quy trình làm việc phức tạp.
  + Khám phá và kết nối với các công cụ và tài nguyên tùy chỉnh.
  + Hiểu và kiểm toán mọi tương tác (**observability**).
  **+ Foundational Services (Dịch vụ Nền tảng):** Các dịch vụ này được phân loại để chạy **agents** an toàn ở quy mô lớn.

  **+ Enhance with tools & memory (Tăng cường với công cụ & bộ nhớ):** Bao gồm Memory, Gateway, Browser tool và Code Interpreter.

  **+ Deploy securely at scale (Triển khai an toàn ở quy mô lớn):** Bao gồm Runtime và Identity.

  **+ Gain operational insights (Thu thập thông tin chi tiết về vận hành):** Bao gồm Observability.

  **+ Enabling Agents at Scale (Architecture) (Kích hoạt Agents ở Quy mô lớn (Kiến trúc)):** Kết nối với AgentCore Gateway (qua MCP), Memory, Identity, Observability, Browser và Code Interpreter.

  **+ Frameworks for Building Agents (Các Framework để Xây dựng Agents):** CrewAI, Google ADK, LangGraph/LangChain, LlamaIndex, OpenAI Agents SDK và Strands Agents SDK.

### Điểm nổi bật Chính
- **Bedrock là Trung tâm GenAI:** Amazon Bedrock cung cấp các Foundation Models được quản lý hoàn toàn từ các công ty hàng đầu cho nhiều tác vụ khác nhau.

- **Tùy chỉnh thông qua Prompts và Dữ liệu:** Nhiều cách khác nhau để tạo prompts (Zero-Shot, Few-shot, CoT) và sử dụng **RAG** để bổ sung thông tin cho các phản hồi tốt hơn của mô hình.

- **Embeddings thúc đẩy Tìm kiếm:** Amazon Titan Embedding là một mô hình nhẹ quan trọng để dịch văn bản sang số, giúp đạt được độ chính xác cao trong các tác vụ truy xuất (như RAG).

- **Mô hình được Đào tạo trước:** AWS cung cấp nhiều dịch vụ AI sẵn sàng sử dụng cho các nhu cầu chung, như **Rekognition** cho hình ảnh và **Textract** cho tài liệu.

- **AgentCore Giải quyết các vấn đề Sản xuất:** Amazon Bedrock AgentCore là nền tảng toàn diện mới xử lý các phần khó khăn trong việc vận hành AI Agents ở quy mô lớn (như Memory, Identity và Observability).

### Áp dụng vào Công việc

- Rất hữu ích trong các dự án sau này của nhóm chúng tôi, có thể bao gồm việc sử dụng nhiều hơn các AI Foundation Models trong kiến trúc của chúng tôi.

### Trải nghiệm Sự kiện
- Các diễn giả nói rất tốt và cung cấp nhiều thông tin
- **Q&A:** Thành viên nhóm đã hỏi một câu hỏi lạc đề nhưng rất quan trọng đối với dự án của chúng tôi
  + **Hỏi:** **SNS** trong kiến trúc của chúng tôi được sử dụng để xử lý Guard Duty Findings đã gặp phải tình huống hơn 1000+ cảnh báo xuất hiện cùng một lúc, làm thế nào để chúng tôi giải quyết việc này?
  + **Đáp:** Thêm **SQS** để xếp hàng các sự kiện và đảm bảo không có cảnh báo nào bị bỏ lỡ
- Lọt **top 10** trong trò chơi **Kahoot Quiz** cuối sự kiện và có một bức ảnh với các diễn giả
- Tạo một nhóm không chính thức: "**Mèo Cam Đeo Khăn**", sự hợp tác chung giữa nhóm của tôi "**The Ballers**" và "**Vinhomies**"

#### Một số hình ảnh sự kiện
![Top 10 Kahoot](/images/4-Event/Kahoottop10.jpg)
_Top 10 Kahoot_
![Một bức ảnh với các diễn giả và 10 người chơi hàng đầu](/images/4-Event/WithSpeaker.jpg)
_Top 10 người chơi_
![Nhóm Mèo Cam Đeo Khăn](/images/4-Event/Meocamdeokhan.jpg)
_Ảnh nhóm Mèo Cam Đeo Khăn_