---
title: "Sự kiện 6"
date: "2025-11-29"
weight: 06
chapter: false
pre: " <b> 4.6. </b> "
---

# Báo Cáo Tóm Tắt: “AWS Cloud Mastery Series #3: AWS Well-Architected – Security Pillar Workshop”

### Mục tiêu Sự kiện

- Giới thiệu **AWS Cloud Club**
- Trụ cột 1: Quản lý Danh tính và Truy cập (**Identity & Access Management - IAM**)
- Trụ cột 2: Phát hiện & Giám sát Liên tục (**Detection & Continuous Monitoring**)
- Trụ cột 3: Bảo vệ Cơ sở hạ tầng (**Infrastructure Protection**)
- Trụ cột 4: Bảo vệ Dữ liệu (**Data Protection**)
- Trụ cột 5: Ứng phó Sự cố (**Incident Response**)

### Diễn giả

- **Le Vu Xuan An** - AWS Cloud Club Captain HCMUTE
- **Tran Duc Anh** - AWS Cloud Club Captain SGU
- **Tran Doan Cong Ly** - AWS Cloud Club Captain PTIT
- **Danh Hoang Hieu Nghi** - AWS Cloud Club Captain HUFLIT

- **Huynh Hoang Long** - AWS Community Builders
- **Dinh Le Hoang Anh** - AWS Community Builders

- **Nguyen Tuan Thinh** - Cloud Engineer Trainee
- **Nguyen Do Thanh Dat** - Cloud Engineer Trainee

- **Van Hoang Kha** - Cloud Security Engineer, AWS Community Builder

- **Thinh Lam** - FCJ Member
- **Viet Nguyen** - FCJ Member

- **Mendel Grabski (Long)** - Ex-Head of Security & DevOps, Cloud Security Solution Architect
- **Tinh Truong** - Platform Engineer at TymeX, AWS Community Builder 

### Điểm nhấn chính

### AWS Cloud Club
Giới thiệu về **AWS Cloud Club**:
- Giúp khám phá và phát triển các kỹ năng **cloud computing**
- Phát triển khả năng lãnh đạo kỹ thuật
- Xây dựng các kết nối có ý nghĩa trên toàn cầu
Cung cấp kinh nghiệm thực hành **AWS (hands-on AWS Experiences)**, cố vấn với các **AWS Professional** và hỗ trợ nghề nghiệp lâu dài

Các **AWS Cloud Club** là thành viên của **FCJA**:
- **AWS Cloud Club HCMUTE**
- **AWS Cloud Club SGU**
- **AWS Cloud Club PTIT**
- **AWS Cloud Club HUFLIT**

Lợi ích: Xây dựng Kỹ năng, Cộng đồng và Cơ hội

### Quản lý Danh tính và Truy cập (IAM)
**IAM** là một dịch vụ **AWS** thiết yếu, chịu trách nhiệm kiểm soát truy cập an toàn. **IAM** quản lý Người dùng (**Users**), Nhóm (**Groups**), Vai trò (**Roles**) và Quyền (**Permissions**), đồng thời đảm bảo cả xác thực (**authentication**) và ủy quyền (**authorization**).

- Các Phương pháp Hay nhất bao gồm:

    + Nguyên tắc Đặc quyền Tối thiểu (**Least Privilege Principle**).

    + Xóa khóa truy cập **root** sau khi tạo.

    + Tránh sử dụng "*" trong **Actions/Resources**

    + Sử dụng **Single Sign-On (SSO)** để tích hợp đa tài khoản và quản lý truy cập tập trung.

- **Service Control Policies (SCPs):** Các chính sách cấp Tổ chức (**Organization-level**) đặt ra các quyền tối đa có sẵn cho tất cả các tài khoản trong một Tổ chức. **SCPs** chỉ lọc quyền; chúng không bao giờ cấp quyền.

- **Permission Boundaries:** Đặt ra các quyền tối đa mà một chính sách dựa trên danh tính (**identity-based policy**) có thể cấp cho một Người dùng/Vai trò cụ thể trong một tài khoản.

- **MFA**:
| TOTP (Mật khẩu Dùng một lần dựa trên Thời gian) | FIDO2 (Fast Identity Online 2) |
| :--- | :--- |
| **Bí mật được chia sẻ** | **Mã hóa khóa công khai** |
| Yêu cầu nhập thủ công mã 6 chữ số | Yêu cầu chạm đơn giản hoặc quét sinh trắc học |
| Miễn phí | Thay đổi |
| Sao lưu và phục hồi linh hoạt | Sao lưu nghiêm ngặt và không phục hồi |

- Xoay vòng Thông tin Xác thực (**Credential Rotation**) với **AWS Secrets Manager**:
    + **Credential Updater** sử dụng các hàm của **Secrets Manager** theo một chu kỳ: Tạo **Secret**, Đặt **Secret** (ví dụ: cứ sau 7 ngày) Kiểm tra **Secret**, và Hoàn tất **Secret**
    + Các sự kiện xoay vòng có thể được gửi đến một **EventBridge Schedule** để kiểm soát thời gian.
    + Cuối cùng loại bỏ **Secret** trước đó

### Phát hiện và Giám sát Liên tục (Detection and Continous Monitoring)
- Khả năng hiển thị Bảo mật Đa lớp (**Multi-Layer Security Visibility**):
    + **Management Events:** Các cuộc gọi **API** và hành động console trên tất cả các tài khoản tổ chức.
    + **Data Events:** Truy cập đối tượng **S3** và thực thi **Lambda** ở quy mô lớn.
    + **Network Activity Events:** Tích hợp **VPC Flow Logs** để giám sát cấp độ mạng.
    + **Organization Coverage:** Ghi nhật ký thống nhất trên tất cả các tài khoản thành viên và khu vực.

- Cảnh báo & Tự động hóa với **EventBridge**

    + **Real-time Events:** Các sự kiện **CloudTrail** chảy đến **EventBridge** để xử lý ngay lập tức. Đây là nền tảng của Kiến trúc Hướng sự kiện (**Event-Driven Architecture - EDA**), cho phép các hệ thống phản ứng với các thay đổi khi chúng xảy ra.

    + **Automated Alerting:** Phát hiện các hoạt động đáng ngờ trên tất cả các tài khoản tổ chức.

    + **Cross-account Event Routing:** Xử lý sự kiện tập trung và phản hồi tự động. **EventBridge** làm cho điều này liền mạch, định tuyến các sự kiện dựa trên các quy tắc tới các đích trên các tài khoản hoặc khu vực.

    + **Integration & Workflows:** Tích hợp **Lambda, SNS, SQS** cho các quy trình làm việc bảo mật tự động.

- Phát hiện dưới dạng Mã (**Detection-as-Code**):
    
    + **CloudTrail Lake Queries:** Tạo và sử dụng các quy tắc phát hiện dựa trên **SQL** cho việc săn tìm mối đe dọa nâng cao (**advanced threat hunting**).

    + **Version-Controlled Logic:** Các quy tắc và logic phát hiện được theo dõi và quản lý thông qua các kho mã.

    + **Automated Deployment:** Các dấu vết và quy tắc phát hiện được triển khai tự động trên tất cả các tài khoản tổ chức liên quan, đảm bảo phạm vi bảo mật đồng nhất.

    + **Infrastructure-as-Code (IaC):** Sử dụng các công cụ **IaC** để thiết lập và cấu hình tự động các dấu vết ghi nhật ký và sự kiện của tổ chức
### Guard Duty
- **GuardDuty** là một giải pháp Phát hiện Mối đe dọa Thông minh, Luôn Bật (**Always-On, Intelligent Threat Detection solution**)

- **GuardDuty** Hoạt động như thế nào – Dựa trên phân tích liên tục của **Three Pillars of Detection** (Ba Trụ cột Phát hiện):

| Nguồn Dữ liệu | Những gì nó Giám sát | Ví dụ Thực tế |
| :--- | :--- | :--- |
| **CloudTrail Events** | Hành động **IAM**, thay đổi quyền, cuộc gọi **API** | Kẻ tấn công vô hiệu hóa ghi nhật ký để che giấu dấu vết. |
| **VPC Flow Logs** | Lưu lượng mạng đến/đi từ tài nguyên của bạn | **EC2** gửi dữ liệu đến máy chủ **C2 botnet**. |
| **DNS Logs** | Truy vấn **DNS** từ cơ sở hạ tầng của bạn | Các truy vấn bị nhiễm **Malware** đến các trang web đào tiền mã hóa. |

- Các Gói Bảo vệ Nâng cao (**Advanced Protection Plans**): **GuardDuty** cung cấp các tiện ích bổ sung phát hiện chuyên biệt để bao phủ toàn diện:

    + **S3 Protection:** Phát hiện các mẫu truy cập **S3** bất thường và quét **malware** trong các đối tượng **S3** tại thời điểm tải lên.

    + **EKS Protection:** Giám sát nhật ký kiểm tra **Kubernetes** để phát hiện truy cập trái phép và xâu chuỗi các phát hiện với **S3** để lập bản đồ đường tấn công đầy đủ.

    + **Malware Protection:** Tự động quét các khối **EBS** của các phiên bản **EC2** khi nghi ngờ bị xâm phạm.

    + **RDS Protection:** Phân tích nhật ký hoạt động đăng nhập vào cơ sở dữ liệu (Aurora/RDS) và phát hiện các cuộc tấn công **brute-force** (nhiều lần đăng nhập thất bại từ một **IP**).

    + **Lambda Protection:** Giám sát nhật ký mạng chảy từ các lời gọi hàm **Lambda** và phát hiện xem một hàm bị xâm phạm có gửi dữ liệu đến các **IP** độc hại hay không.

    + **Runtime Monitoring – Deep Inside Your OS (Giám sát Runtime – Sâu bên trong OS của bạn):** Đạt được bằng cách sử dụng một **GuardDuty Agent** được cài đặt trên **EC2/EKS/ECS Fargate**. Nó giám sát các quy trình đang chạy, các mẫu truy cập tệp, các cuộc gọi hệ thống và các nỗ lực leo thang đặc quyền (**privilege escalation**) hoặc **reverse shells**.

- Các Tiêu chuẩn Tuân thủ (**Compliance Standards**):

    + **AWS Foundational Security Best Practices:** Được phát triển bởi **AWS** và bao gồm một loạt các dịch vụ **AWS**.

    + **CIS AWS Foundations Benchmark:** được phát triển bởi: **AWS** và các chuyên gia trong ngành tập trung vào Danh tính (**IAM**), Ghi nhật ký & Giám sát, và Mạng.

- Thực thi Tuân thủ với **Detection-as-Code**

    + **IaC Tool:** **AWS CloudFormation** được sử dụng để triển khai cấu hình.

    + **Compliance Engine:** **AWS CloudFormation** đẩy các kiểm tra cấu hình đến **AWS Security Hub CSPM**.

    + **Compliance Standards Applied:** **Security Hub** thực hiện kiểm tra đối với các tiêu chuẩn được liệt kê (**AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark, PCI DSS, NIST**).

    + **Resources Covered:** **Amazon S3**, **Amazon EC2**, và **Amazon RDS**.

### Kiểm soát Bảo mật Mạng (Network Security Controls)

- Vector Tấn công: Các mối đe dọa được phân loại thành Tấn công Vào (**Ingress Attacks**) (**DDoS, SQL injection**), Tấn công Ra (**Egress Attacks**) (**data exfiltration, DNS tunneling**), và Tấn công Bên trong (**Inside Attacks**) (**lateral movement**).

- **Security Groups (SG):** Hoạt động như một **Stateful firewall** ở cấp độ phiên bản/giao diện. Chúng chỉ hỗ trợ các quy tắc cho phép (**allow rules**) và có một từ chối tất cả ngầm định (**implicit deny all**).

- **Network ACLs (NACLs):** Hoạt động ở cấp độ **subnet**. Chúng là **stateless** và sử dụng các quy tắc được đánh số để cho phép (**ALLOW**) hoặc từ chối (**DENY**) lưu lượng truy cập một cách rõ ràng.

- **AWS TGW Security Group Referencing:** Cho phép các **Transit Gateway (TGW) VPCs** xác định các quy tắc Đến (**Inbound rules**) chỉ bằng cách sử dụng các tham chiếu **SG**.

- **Route 53 Resolver:** Định tuyến các truy vấn **DNS** đến **Private DNS** (**private hosted zones**), **VPC DNS**, hoặc **Public DNS**.

- **AWS Network Firewall:**

    + Trường hợp Sử dụng: Lọc lưu lượng ra (**Egress filtering**) (chặn các miền xấu, các giao thức độc hại), Phân đoạn môi trường (**Environment segmentation**) (**VPC to VPC**), và Ngăn chặn Xâm nhập (**Intrusion prevention**) (quy tắc **IDS/IPS**).

    + Phòng thủ Chủ động (**Active Defense**): Có thể tự động chặn lưu lượng truy cập độc hại bằng cách sử dụng **Amazon Threat Intelligence**, nơi các phát hiện của **GuardDuty** được đánh dấu để chặn tự động.

### Bảo vệ & Quản trị Dữ liệu (Data Protection & Governance)

- Mã hóa (**Encryption - KMS**): Dữ liệu được mã hóa bằng Khóa Dữ liệu (**Data Key**), được bảo vệ bởi Khóa Chính (**Master Key - CMK**). Các chính sách **KMS** thực thi lớp bảo mật thứ hai bằng Khóa Điều kiện (**Condition keys**) để xác định Khi nào mã hóa/giải mã được cho phép.

- Quản lý Chứng chỉ (**Certificate Management - ACM**): Cung cấp chứng chỉ công khai miễn phí và tự động gia hạn chứng chỉ 60 ngày trước khi hết hạn. Xác thực **DNS** là phương pháp xác thực được khuyến nghị.

- **Secrets Manager:** Giải quyết vấn đề thông tin xác thực được mã hóa cứng (**hardcoded credentials**). Nó sử dụng logic **Lambda** 4 bước (**createSecret, setSecret, testSecret, finishSecret**) để tự động xoay vòng thông tin xác thực mà không bị gián đoạn.

- Bảo mật Dịch vụ **API** (**S3 & DynamoDB**): **S3** yêu cầu **TLS 1.2+** và chính sách **bucket** với **aws:SecureTransport** để thực thi. **DynamoDB** an toàn theo mặc định với **HTTPS** bắt buộc.

- Bảo mật Dịch vụ Cơ sở dữ liệu (**Database Service Security - RDS**): Yêu cầu tin cậy phía máy khách trong **AWS Root CA Bundle** để xác minh danh tính máy chủ và thực thi phía máy chủ (ví dụ: **rds.force_ssl=1** cho **PostgreSQL**).

### Ứng phó & Phòng ngừa Sự cố (Incident Response & Prevention)

- Các Phương pháp Hay nhất về Phòng ngừa: Các bước chính bao gồm sử dụng thông tin xác thực tạm thời, không bao giờ để lộ **S3 buckets** trực tiếp, đặt các dịch vụ nhạy cảm phía sau các **private subnets**, quản lý mọi thứ thông qua **Infrastructure as Code**, và sử dụng cổng đôi cho các thay đổi rủi ro cao (phê duyệt **PR**, triển khai **pipeline**).

- Quy trình Ứng phó Sự cố: Một cách tiếp cận có cấu trúc gồm 5 bước: Chuẩn bị, Phát hiện & Phân tích, Ngăn chặn (cô lập, thu hồi thông tin xác thực), Tiêu diệt & Phục hồi, và Sau Sự cố (**Post-Incident**) (bài học kinh nghiệm).

### Trải nghiệm Sự kiện

- Sự kiện này cực kỳ hữu ích cho nhóm chúng tôi, phù hợp trực tiếp với dự án **Automated Incident Response and Forensics** của chúng tôi

- **Hỏi:** Dự án của nhóm chúng tôi là một công cụ **Automated Incident Response and Forensics** với **Guard Duty** là trọng tâm chính trong việc ứng phó sự cố, nhưng qua thử nghiệm chúng tôi thấy rằng **Guard Duty** có thể mất tới 5 phút để tạo ra một **finding** khi sự cố xảy ra, chúng tôi muốn hỏi có giải pháp nào để giảm độ trễ này không?

- **Đáp:** Việc **Guard Duty** mất 5 phút để tạo ra các **findings** là điều bạn phải chấp nhận, vì đó là cách **Guard Duty** được cấu hình để hoạt động: **Guard Duty** phải đi qua một lượng lớn bộ dữ liệu bảo mật để xác định mối đe dọa chính xác và sau đó tạo ra một **finding**. Tuy nhiên, nếu bạn muốn giảm độ trễ, một trong những cách bạn giải quyết nó là với tích hợp dịch vụ bảo mật của bên thứ ba như: **Open Clarity Free** để có các **findings** gần như thời gian thực và bạn cũng có thể phát hiện các bất thường và hành vi người dùng bất thường với **CloudTrail**.

- **Mr.Mendel Grabski** đã rất nhiệt tình đề nghị hỗ trợ khi chúng tôi hỏi về dự án của mình sau sự kiện

#### Một số hình ảnh sự kiện

![All Attendee Picture](/images/4-Event/Event6AttendeePic.jpg)
_Hình ảnh tất cả Người tham dự_

![Group Picture With Speaker Mendel Grabski and Speaker Van Hoang Kha](/images/4-Event/Event6PicturewithSpeakers.jpg)
_Ảnh nhóm với Diễn giả Mendel Grabski và Diễn giả Van Hoang Kha_