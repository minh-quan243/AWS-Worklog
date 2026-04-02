---
title: "Sự kiện 2"
date: "2025-10-03"
weight: 02
chapter: false
pre: " <b> 4.2. </b> "
---

# Báo Cáo Tóm Tắt: “AI-Driven Development Life Cycle: Reimagining Software Engineering”

### Mục tiêu Sự kiện

- Khám phá sự chuyển đổi mang tính cách mạng trong phát triển phần mềm được thúc đẩy bởi **generative AI** (AI tạo sinh).
- Giới thiệu **AI-Driven Development Life Cycle (AI-DLC)** và các khái niệm cốt lõi của nó.
- Demo về **Kiro** và **Amazon Q Developer**.

### Diễn giả

- **Toan Huynh** – Specialist SA, PACE
- **My Nguyen** - Sr. Prototyping Architect, Amazon Web Services - ASEAN


### Nội dung nổi bật

- Tập trung vào khái niệm **AI-DLC**, một khuôn khổ nơi **AI** điều phối quá trình phát triển, bao gồm lập kế hoạch, phân rã tác vụ và đề xuất kiến trúc, trong khi các nhà phát triển vẫn giữ trách nhiệm cuối cùng về xác thực, ra quyết định và giám sát.

**- Khái niệm Cốt lõi của AI-DLC:** Phương pháp tiếp cận này lấy con người làm trung tâm (**Human-Centric**), với **AI** đóng vai trò là Cộng tác viên (**Collaborator**) để tăng cường khả năng của nhà phát triển, dẫn đến Tốc độ Giao hàng Nhanh hơn (**Accelerated Delivery**) (chu kỳ được tính bằng giờ/ngày thay vì tuần/tháng).

**- Quy trình làm việc của AI-DLC:** Đây là một vòng lặp lặp đi lặp lại bao gồm **AI Tasks** (Tạo kế hoạch, Thực hiện kế hoạch, Tìm kiếm làm rõ) và **Human Tasks** (Cung cấp làm rõ, Thực hiện kế hoạch), trong đó **AI** liên tục đặt các câu hỏi làm rõ và chỉ thực hiện giải pháp sau khi có sự xác thực của con người.

**- Các giai đoạn của AI-DLC:** Vòng đời được chia thành Khởi tạo (**Inception**), Xây dựng (**Construction**) và Vận hành (**Operation**). Mỗi giai đoạn xây dựng ngữ cảnh phong phú hơn cho giai đoạn tiếp theo:

+ **Inception:** Bao gồm xây dựng ngữ cảnh, làm rõ ý định bằng **User Stories** (Câu chuyện người dùng), và lập kế hoạch với **Units of Work** (Đơn vị công việc).

+ **Construction:** Bao gồm Lập mô hình Miền (**Domain Modeling**), tạo mã và kiểm thử, thêm các thành phần kiến trúc, và triển khai bằng **IaC** & kiểm thử.

+ **Operation:** Tập trung vào triển khai vào sản xuất và quản lý sự cố (**incidents**).

**- Các Thách thức mà AI-DLC nhằm mục đích Giải quyết:**

+ **Mở rộng phát triển AI:** Các công cụ lập trình AI có thể thất bại với các dự án phức tạp.

+ **Kiểm soát hạn chế:** Các công cụ hiện có gây khó khăn trong việc cộng tác và quản lý các **AI agent**.

+ **Chất lượng mã:** Việc duy trì kiểm soát chất lượng khi chuyển từ **proof-of-concept** sang sản xuất trở nên khó khăn.

## Khám phá Sâu hơn: Kiro - AI IDE cho Prototype đến Production

- **Kiro**, một Môi trường Phát triển Tích hợp (**IDE**) ưu tiên **AI** (**AI-first**), hỗ trợ **AI-DLC**, tập trung vào Phát triển dựa trên Đặc tả (**Spec-driven development**).

**- Phát triển dựa trên Đặc tả:** **Kiro** biến một lời nhắc cấp cao (ví dụ: "Tôi muốn tạo một ứng dụng chat như Slack") thành các yêu cầu rõ ràng (**requirements.md**), thiết kế hệ thống (**design.md**), và các tác vụ riêng biệt (**tasks.md**), về cơ bản chuyển đổi việc phát triển từ "vibe coding" sang một quy trình có cấu trúc, có thể truy vết. Các nhà phát triển cộng tác với **Kiro** dựa trên các đặc tả này, vốn đóng vai trò là nguồn sự thật duy nhất (**source of truth**).

**- Quy trình làm việc của Agentic:** Các **AI agent** của **Kiro** thực hiện đặc tả trong khi vẫn giữ nhà phát triển con người kiểm soát, với các tính năng chính là:

**+ Implementation Plan (Kế hoạch Thực hiện):** **Kiro** tạo ra một Kế hoạch Thực hiện chi tiết với các tác vụ bắt đầu, các tác vụ phụ (ví dụ: "Implement user registration and login endpoints," "Implement JWT middleware"), và liên kết chúng trở lại các yêu cầu cụ thể để xác thực.

**+ Agent Hooks:** Chúng ủy quyền các tác vụ cho **AI agent** kích hoạt trên các sự kiện như "lưu file". Chúng tự động thực thi trong nền dựa trên các lời nhắc được xác định trước, giúp mở rộng công việc bằng cách tạo tài liệu, kiểm thử đơn vị (**unit tests**) hoặc tối ưu hóa hiệu suất mã.
### Những điểm rút ra chính
**- AI Đảm bảo Khả năng Sẵn sàng cho Sản xuất:** **Kiro** tạo các tài liệu thiết kế chi tiết (như sơ đồ luồng dữ liệu và API contracts) và tạo **unit tests** trước khi mã được viết, đảm bảo rằng mã được tạo bởi **AI** đã sẵn sàng cho sản xuất và dễ bảo trì, chứ không chỉ là một **prototype** nhanh chóng.

**- Kiểm soát của Con người thông qua Artifacts:** Các nhà phát triển duy trì quyền kiểm soát không phải bằng cách viết phần lớn mã, mà bằng cách xác thực và tinh chỉnh các **artifacts**—các yêu cầu, thiết kế và kế hoạch tác vụ—trước khi các **AI agent** thực hiện triển khai.

### Áp dụng vào Công việc

**- Tích hợp Amazon Q Developer/Các Công cụ Tương tự:** Tích hợp các trợ lý lập trình **AI** vào các dự án học thuật của tôi để tự động hóa mã **boilerplate** và các tác vụ thông thường nhằm tăng năng suất.

**- Tập trung vào các Tác vụ Giá trị Cao:** Bằng cách để **AI** tự động hóa những công việc nặng nhọc không tạo ra sự khác biệt (**undifferentiated heavy lifting**), tôi có thể tập trung thời gian vào việc nắm vững các tác vụ sáng tạo, giá trị cao hơn như Lập mô hình Miền (**Domain Modeling**) và Thiết kế Kiến trúc (**Architectural Design**), đây là những hoạt động lấy con người làm trung tâm quan trọng trong giai đoạn Xây dựng (**Construction**).  

### Trải nghiệm Sự kiện

Tham dự sự kiện **AI-Driven Development Life Cycle: Reimagining Software Engineering** đã mang lại một cái nhìn hấp dẫn về tương lai của phát triển phần mềm. Rõ ràng là **Generative AI** không chỉ là một trợ lý lập trình; nó sẵn sàng trở thành một công cụ điều phối cốt lõi cho toàn bộ quá trình phát triển. Phiên này được cấu trúc tốt, chuyển từ khái niệm bao quát của **AI-DLC** sang các minh chứng cụ thể về **Amazon Q Developer** và **Kiro**. Bản demo về **Kiro** đặc biệt ấn tượng, cho thấy cách một lời nhắc văn bản đơn giản có thể được chuyển đổi thành một kế hoạch phát triển đầy đủ, có thể thực thi và truy vết ngay trong **IDE**.

#### Bài học rút ra
- Ba thách thức chính đối với phát triển **AI** hiện tại (mở rộng, kiểm soát hạn chế và chất lượng mã) khiến phương pháp tiếp cận có cấu trúc, được con người xác thực của **AI-DLC** trở nên rất cần thiết và được cân nhắc kỹ lưỡng.


#### Một số hình ảnh sự kiện
![Group picture during the event](/images/4-Event/TheBois-AIDLC.jpg)
_Ảnh nhóm_