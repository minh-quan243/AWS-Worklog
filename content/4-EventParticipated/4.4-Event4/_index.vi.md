---
title: "Sự kiện 4"
date: "2025-11-17"
weight: 04
chapter: false
pre: " <b> 4.4. </b> "
---

# Báo Cáo Tóm Tắt: “AWS Cloud Mastery Series #2 - DevOps on AWS”

### Mục tiêu Sự kiện

- Giới thiệu các Dịch vụ DevOps của **AWS** – **CI/CD Pipeline**
- Giới thiệu **Infrastructure as Code (IaC)** và các công cụ liên quan
- Giới thiệu các Dịch vụ Container trên **AWS**
- Đảm bảo khả năng Giám sát và Khả năng Quan sát (**Monitoring and Observability**) bằng cách sử dụng các Dịch vụ **AWS**

### Diễn giả

- **Truong Quang Tinh** – AWS Community Builder, Platform Engineer - TymeX
- **Bao Huynh** – AWS Community Builder
- **Nguyen Khanh Phuc Thinh** – AWS Community Builder
- **Tran Dai Vi** – AWS Community Builder
- **Huynh Hoang Long** – AWS Community Builder
- **Pham Hoang Quy** – AWS Community Builder
- **Nghiem Le** – AWS Community Builder
- **Dinh Le Hoang Anh** - Cloud Engineer Trainee, First Cloud AI Journey

### Điểm nhấn chính

### Tư duy DevOps (DevOps Mindset)

**- Văn hóa (Culture):** Cộng tác, Tự động hóa, Học hỏi và Đo lường Liên tục (**Collaboration, Automation, Continuous Learning and Measurement**)
**- Các Vai trò DevOps:** DevOps Engineer, Cloud Engineer, Platform Engineer, Site Reliability Engineer
**- Các Chỉ số Thành công (Success Metrics):**
    + Đảm bảo tình trạng triển khai (**deployment health**)
    + Cải thiện tính linh hoạt (**agility**)
    + Độ ổn định hệ thống (**System stability**)
    + Tối ưu hóa Trải nghiệm Khách hàng (**Optimize customer Experience**)
    + Chứng minh giá trị các khoản đầu tư công nghệ (**Justify technology investments**)

| NÊN LÀM (DO) | KHÔNG NÊN LÀM (DON'T) |
| :--- | :--- |
| Bắt đầu với các Khái niệm Cơ bản (Fundamentals) | Mắc kẹt trong Vòng lặp Tutorial (Tutorial Hell) |
| Học bằng cách Xây dựng các Dự án Thực tế | Sao chép-dán một cách mù quáng (Copy-paste blindly) |
| Tài liệu hóa Mọi thứ (Document Everything) | So sánh Tiến độ của Bản thân với Người khác |
| Thành thạo từng thứ một | Bỏ cuộc Sau khi Thất bại |
| Tăng cường Kỹ năng Mềm (Soft Skills Enhancement) | |

**- Tích hợp Liên tục (Continuous Integration):** Các thành viên trong nhóm tích hợp công việc của họ thường xuyên, nhằm mục đích Cung cấp Liên tục (**Continuous Delivery**) và Triển khai Liên tục (**Deployment**)

### Cơ sở hạ tầng dưới dạng Mã (Infrastructure as Code - IaC)

**- Lợi ích:** Tự động hóa, Khả năng Mở rộng, Khả năng Tái tạo (**Reproducibility**) và Cộng tác tốt hơn

#### AWS CloudFormation

Công cụ **IaC** tích hợp sẵn của **AWS**, sử dụng các template được viết bằng **YAML** hoặc **JSON**, có thể xây dựng mọi Cơ sở hạ tầng **AWS** một cách tự động

**- Stack:** Một tập hợp các Tài nguyên **AWS** được định nghĩa trong một template, có thể được **CloudFormation** sử dụng để tạo, cập nhật hoặc xóa các tài nguyên đó

**- CloudFormation Template:** Một tệp **YAML/JSON** định nghĩa Cơ sở hạ tầng **AWS**, hoạt động như một bản thiết kế (**blueprint**) để triển khai và cấu hình tài nguyên

**- Cách thức hoạt động:** Tạo template -> Lưu trữ trong S3 Bucket hoặc bộ nhớ cục bộ -> Sử dụng **CloudFormation** để tạo **Stacks** dựa trên template -> **CloudFormation** xây dựng tài nguyên

**- Drift Detection (Phát hiện Lệch):** Phát hiện các thay đổi trong cơ sở hạ tầng so với **Stack** => Cập nhật **Stack** hoặc hoàn nguyên thay đổi, hữu ích cho việc quản lý phiên bản (**versioning**)

#### AWS Cloud Development Kit (CDK)

Khung phát triển phần mềm mã nguồn mở, hỗ trợ **IaC** bằng cách sử dụng các ngôn ngữ lập trình thực tế (**Python, Java, C#.Net, Type/JavaScript và Go**)

**- Construct:** Các khối xây dựng, bao gồm các thành phần đại diện cho Tài nguyên **AWS** và Cấu hình của chúng, có 3 cấp độ **construct**:
  + **L1 Construct:** Tài nguyên cấp thấp ánh xạ trực tiếp tới một tài nguyên **AWS CloudFormation** duy nhất
  + **L2 Construct:** Cung cấp mức độ trừu tượng cao hơn thông qua **API** dựa trên ý định trực quan (**intuitive intent-based API**), đóng gói các phương pháp hay nhất và các mặc định bảo mật
  + **L3 Construct:** Các mẫu kiến trúc hoàn chỉnh với nhiều tài nguyên, triển khai có ý kiến chuyên môn (**opinionated implementation**) và triển khai nhanh chóng

#### AWS Amplify

Nền tảng **AWS** giúp dễ dàng xây dựng, triển khai và mở rộng các ứng dụng web và di động, sử dụng **CloudFormation** bên dưới: Các **Stacks** được triển khai để xây dựng cơ sở hạ tầng theo chương trình (**programmatically**)

#### Terraform

Công cụ **IaC**, bắt đầu bằng việc định nghĩa cơ sở hạ tầng bằng mã **Terraform** và lập kế hoạch sau đó áp dụng cơ sở hạ tầng trên nhiều nền tảng **cloud** như **Azure, AWS, Google Cloud**, v.v.

**- Điểm mạnh:** Hỗ trợ Đa đám mây (**Multi-Cloud**), Theo dõi Trạng thái (**State tracking**) với cùng một cấu hình

#### Cách chọn Công cụ IaC?
**- Tiêu chí:**
  + Lập kế hoạch sử dụng một **Cloud** hay nhiều **Cloud**?
  + Vai trò là Developer hay Ops?
  + **Cloud** và Hệ sinh thái có hỗ trợ công cụ đó không?

### Dịch vụ Container trên AWS

#### Dockerfile

Một **Dockerfile** định nghĩa cách xây dựng một **container image**, mô tả môi trường, các phụ thuộc, các bước xây dựng và cấu hình thời gian chạy cuối cùng, đảm bảo rằng ứng dụng chạy nhất quán trên bất kỳ hệ thống nào hỗ trợ **Dockers**

**- Images:** Một bản thiết kế đóng gói của một ứng dụng, được xây dựng từ một **Dockerfile** bằng cách sử dụng hệ thống tệp phân lớp (**layered file system**), được sử dụng để tạo các **container** nhất quán giữa các môi trường

**- Quy trình làm việc:** **Dockerfile** xây dựng **Docker Image**, sau đó có thể được sử dụng để chạy **Container** và đẩy lên **ECR/Docker Hub**

#### Amazon ECR

Một **container registry** được quản lý hoàn toàn, giúp dễ dàng lưu trữ, quản lý và chia sẻ hình ảnh **Docker container** một cách an toàn.
**Container registry** riêng tư, an toàn và có khả năng mở rộng của **AWS**

**- Tính năng:**
  + Quét hình ảnh (**Image Scanning**)
  + Thẻ Bất biến (**Immutable Tags**)
  + Chính sách Vòng đời (**Lifecycle Policies**)
  + Mã hóa & **IAM**

**- Điều phối (Orchestration):** Điều phối nhiều quy trình **container**: khởi động lại **container**, tự động mở rộng quy mô khi tải cao, phân phối lưu lượng truy cập hiệu quả, quản lý nơi đặt và chạy **container**

#### Kubernetes
Mã nguồn mở, tự động hóa việc triển khai, mở rộng quy mô, tự phục hồi (**healing**) và cân bằng tải (**load balancing**)
**- Các thành phần:**
  + **Master Node:** **Control Plane**, quản lý **worker nodes** và **pods**
  + **Worker Node:** Chạy khối lượng công việc ứng dụng bên trong **pods**
  + **Pod:** Đơn vị triển khai nhỏ nhất, có thể chứa một hoặc nhiều **container**
  + **Service**

ECS so với EKS

| Tính năng | Amazon ECS (Elastic Container Service) | Amazon EKS (Elastic Kubernetes Service) |
| :--- | :--- | :--- |
| **Công nghệ Cốt lõi** | Điều phối **container** gốc **AWS** (**AWS-native container orchestration**) | Dựa trên **Kubernetes** (tiêu chuẩn mã nguồn mở) |
| **Độ phức tạp** | Đơn giản hơn, dễ vận hành hơn | Rất linh hoạt nhưng **phức tạp** hơn |
| **Kiến thức Yêu cầu** | **Không cần kiến thức về Kubernetes** | Yêu cầu **kiến thức về Kubernetes** (pods, deployments, v.v.) |
| **Tích hợp AWS** | Tích hợp sâu với **AWS** (ALB, IAM, CloudWatch, v.v.) | Tích hợp **Kubernetes** tiêu chuẩn |
| **Trường hợp Sử dụng/Lợi ích** | Tuyệt vời cho **triển khai nhanh** & **chi phí vận hành thấp hơn** | Tính **di động đa cluster**, **đa đám mây** |
| **Hệ sinh thái/Cộng đồng** | Cộng đồng và công cụ gốc **AWS** | **Hệ sinh thái & công cụ cộng đồng lớn hơn** |
| **Tóm tắt** | ECS = dễ hơn, chạy nhanh hơn, **chi phí vận hành thấp hơn** | EKS = linh hoạt hơn, kiểm soát nhiều hơn, **phức tạp hơn** |

#### App Runner

Thích hợp cho việc triển khai nhanh các ứng dụng web và **REST API**, lý tưởng cho khối lượng công việc sản xuất từ nhỏ đến trung bình

### Giám sát & Khả năng Quan sát (Monitoring & Observability)

#### CloudWatch
- Giám sát Tài nguyên **AWS** và Ứng dụng chạy trên **AWS** theo thời gian thực
- Cung cấp khả năng quan sát
- Báo động (**Alarms**) và phản hồi tự động
- **Dashboard** giúp tối ưu hóa vận hành và chi phí

**- CloudWatch metrics:** Dữ liệu về hiệu suất của hệ thống trên **AWS** hoặc tại chỗ với **CloudWatch Agent**, tích hợp tốt với **EventBridge**, **Auto Scaling** và quy trình làm việc **DevOps**

#### AWS X-Ray
**- Truy vết Phân tán (Distributed Tracing):** Theo dõi các yêu cầu từ đầu đến cuối, và vẽ bản đồ cùng đường dẫn giữa các dịch vụ đã truy cập, thêm **SDK** vào mã để truy vết **IDs**

**- Thông tin chi tiết về Hiệu suất (Performance Insight):** Phân tích nguyên nhân gốc rễ cho độ trễ và lỗi, suy luận thông tin chi tiết từ các dấu vết và cung cấp Giám sát Người dùng Thật (**Real User Monitoring**)

### Trải nghiệm Sự kiện

Sự kiện này rất quan trọng đối với dự án của chúng tôi vì nó giải quyết kế hoạch bổ sung **IaC** bằng cách sử dụng **CDK**, thay vì sử dụng **ClickOps** để duy trì và tái tạo. Ngoài ra, một số thông tin chi tiết hơn về **CloudWatch** đã giúp ích rất nhiều cho tính năng giám sát dữ liệu của chúng tôi

Các diễn giả đã trả lời câu hỏi của nhóm chúng tôi:

- **Hỏi:** Dự án của chúng tôi cho đến nay chỉ được xây dựng bằng **ClickOps**, và chúng tôi đang có kế hoạch sử dụng **CDK**. Có công cụ nào có thể quét và biến cơ sở hạ tầng hiện có của chúng tôi thành **CDK** hoặc **CloudFormation** thay vì phải tái tạo lại cơ sở hạ tầng từ đầu bằng **IaC** không?
- **Đáp:** Rất tiếc là chưa có công cụ nào có thể hỗ trợ vấn đề đó, nhóm của bạn sẽ phải xây dựng lại cơ sở hạ tầng từ đầu. Nếu có bất kỳ cơ hội nào mà bạn tìm thấy một công cụ có thể hỗ trợ, hãy chia sẻ với chúng tôi.

- **Hỏi:** Chúng tôi nhận thấy rằng **AWS X-Ray** được sử dụng với **CloudWatch** tương tự như **CloudTrail** trong phương pháp truy vết của nó, bạn có thể giải thích thêm về điểm khác biệt giữa chúng không?
- **Đáp:** **X-Ray** được sử dụng cho **CloudWatch** và dùng để truy vết các tài nguyên và dịch vụ mà hệ thống tương tác, trong khi **CloudTrail** thường được sử dụng để truy vết các hành động của người dùng **AWS**.

- **Hỏi:** Dự án của chúng tôi được xây dựng xoay quanh **Guard Duty Findings**, bạn có kinh nghiệm nào về cách kích hoạt **Findings** một cách đáng tin cậy cho các kịch bản demo không?
- **Đáp:** Theo kinh nghiệm của tôi, tôi biết rằng **Guard Duty Findings** có thể được kích hoạt bằng các hoạt động quét cổng (**port scanning activities**) nhưng tôi chắc chắn rằng cũng có những cách khác.
- **Đáp:** **Guard Duty** có thể được cấu hình để có một danh sách mối đe dọa (**threat list**) chứa các quy tắc tùy chỉnh để kích hoạt **findings** khi có các hoạt động liên quan đến các miền hoặc **IP** độc hại đã được cấu hình.

Sự kiện này cũng là lần đầu tiên một số diễn giả trình bày một chủ đề:
- Các phần **DevOps** và **IaC** được trình bày tốt
- Phần **Monitoring & Observability** không được tốt bằng và chúng tôi có thể nhận thấy sự lo lắng của diễn giả nhưng vẫn truyền tải được những giá trị tuyệt vời.

#### Một số hình ảnh sự kiện
![Group picture during the event taken by speaker Tran Dai Vi](/images/4-Event/CM2GroupPic.jpg)
_Ảnh nhóm chụp bởi Tran Dai Vi_