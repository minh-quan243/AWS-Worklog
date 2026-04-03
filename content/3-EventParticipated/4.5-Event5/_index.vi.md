---
title: "Sự kiện 5"
date: "2025-11-19"
weight: 05
chapter: false
pre: " <b> 4.5. </b> "
---

# Báo Cáo Tóm Tắt: “Secure Your Applications: AWS Perimeter Protection Workshop”

### Mục tiêu Sự kiện

- Giới thiệu **CloudFront** như nền tảng cho việc bảo vệ vòng ngoài (**perimeter protection**)
- Giới thiệu **WAF** và Bảo vệ Ứng dụng (**Application Protection**)
- Workshop Thực hành: Tối ưu hóa ứng dụng web với **CloudFront**
- Workshop Thực hành: Bảo mật Ứng dụng Web Internet

### Diễn giả

- **Nguyen Gia Hung** - Head of Solution Architect
- **Julian Ju** - Senior Edge Services Specialist Solution Architect
- **Kevin Lim** - Senior Edge Services Specialist GTM

### Điểm nhấn chính

### CloudFront

#### Hình thức Bảo mật Khách hàng cần và Giải pháp

Chân dung Khách hàng (**Customer Personas**):
- Chủ sở hữu trang web nhỏ: Bảo vệ bảo mật cơ bản
- Người dùng Doanh nghiệp: Bảo mật toàn diện chống lại **DDoS** và **bots**
- Doanh nghiệp đang mở rộng quy mô: Khả năng cấu hình nâng cao như **origin failover** và giảm tải **origin** (**origin load reduction**)

Giải pháp: **CloudFront**
- Bảo mật với chi phí hàng tháng có thể dự đoán: Trả giá cố định cho **CDN, WAF, DDoS Protection, DNS** và **Storage**
- Lựa chọn gói đơn lẻ

#### Chính sách Giá CloudFront Mới: CloudFront Flat-Rate

- Giá cố định cho **CDN, WAF, DDoS Protection, DNS** và **Storage**, trả cùng một mức giá cho việc sử dụng không giới hạn

- **AWS** cung cấp 4 gói giá cố định cho **CloudFront**:
  + Free: $0/Tháng
  + Pro: $15/Tháng
  + Business: $200/Tháng
  + Premium: $1000/Tháng

- Mỗi gói có giới hạn sử dụng riêng hàng tháng:
  + Free: 1 triệu yêu cầu + 100 GB Dữ liệu
  + Pro: 10 triệu yêu cầu + 50 TB Dữ liệu
  + Business: 125 triệu yêu cầu + 50 TB Dữ liệu
  + Premium: 500 triệu yêu cầu + 50 TB Dữ liệu

- Vượt quá giới hạn sử dụng => Không phát sinh chi phí nhưng **CloudFront** sẽ giảm hiệu suất và gửi cảnh báo

#### Cách CloudFront giúp bảo vệ vòng ngoài (perimeter protection)
- Chống lại các cuộc tấn công phân tán bằng phòng thủ phân tán: Thay vì phòng thủ các cuộc tấn công từ khắp nơi trên thế giới cùng một lúc, **CloudFront** giúp bằng cách phòng thủ tại các vị trí biên (**edge locations**) gần nhất với nguồn tấn công
- **Shield Advanced:** Có được khả năng hiển thị các cuộc tấn công lớp hạ tầng, quyền truy cập vào Đội ngũ Phản ứng **Shield (Shield Response Team)** 24/7
- Tấn công Volumetric: **CloudFront** cung cấp giảm thiểu nội tuyến (**inline mitigation**), **STN Proxy** bảo vệ khỏi tấn công **SYN flood** và định tuyến mạng tự động

#### Tối ưu hóa chi phí với Amazon CloudFront

- Truyền dữ liệu ra khỏi **AWS** (**AWS Data transfer out**) tới **CloudFront**: Miễn phí
- Nén **HTTP** (**HTTP compression**): Nén tệp đối tượng khoảng 80%
- Mã hóa **HTTPS** trong quá trình truyền (**in transit**):
    + **TLS** độc quyền của **AWS** (**AWS proprietary TLS**)
    + Cấp và xác thực chứng chỉ **TLS** tự động mà không mất thêm chi phí
    + Chuyển hướng tự động từ **HTTP** sang **HTTPS**
    + Phiên bản **TLS** và bộ mã hóa được quản lý bằng chính sách bảo mật
    + **TLS 1.3, ECDSA cert, post quantum**
- Hỗ trợ **mutual TLS** sắp ra mắt
- Che giấu **Origin** (**Origin cloaking**) với **Origin Access Control**: ký yêu cầu bằng thông tin xác thực ngắn hạn, áp dụng cho **S3 Bucket, Lambda Function URL, Elemental Media Package**
- Che giấu **Origin** cho các **custom origins**: Chỉ cho phép **CloudFront IP** với danh sách tiền tố được quản lý (**managed prefixes list**) và thêm **custom header** với một bí mật được xác định trước
- Bảo vệ nội dung với **Signed URL**: Ngăn chặn hành vi sao chép-dán **URL** đánh cắp nội dung
- Lợi ích của Bộ nhớ đệm (**Caching benefits**) để cải thiện tính khả dụng: Bộ nhớ đệm nội dung dựa trên **TTL**, phản hồi mà không cần yêu cầu đến **origin** và phản hồi nội dung cũ (**stale content**) trong trường hợp **origin** hết thời gian chờ
- **Failover** tích hợp sẵn và **origin failover**: Tự động định tuyến lưu lượng truy cập đến vị trí biên tối ưu và khỏe mạnh, điều tương tự cũng xảy ra với **origin** khi **origin** chính bị lỗi => **failover** sang **origin** phụ
- Thất bại duyên dáng (**Graceful failure**): Hỗ trợ trang lỗi tùy chỉnh, nội dung cũ và kết quả lỗi được lưu trong bộ nhớ đệm

#### Hiệu suất nâng cao với Amazon CloudFront

- Kiến trúc bộ nhớ đệm đa lớp (**Multi-layer caching architecture**): Tập hợp lưu lượng truy cập từ các **CloudFront PoPs** trong **Regional Edge Caches** và **Origin Shield**, cho phép gộp yêu cầu (**request collapsing**) và cải thiện tỷ lệ truy cập bộ nhớ đệm (**cache hit ratio**)
- **Multiplexing:** Tải xuống song song
- Kết nối bền bỉ với **origins** (**Persistent connections to origins**): Tránh bắt tay **TCP** bổ sung và duy trì dung lượng **TCP** đầy đủ
- **AWS Global backbone:** Độ trễ thấp hơn và không bị ảnh hưởng bởi sự kiện internet công cộng
- Chạy logic tại biên (**Run logic at the edge**):
    + Chuyển hướng tại biên (**Redirect at edge**) (**CloudFront Function, Lambda at Edge**)
    + Viết lại **URL** / Thử nghiệm **AB Testing** / Chuyển hướng Địa lý (**Geographic Redirect Device-based Redirect**)
    + Cung cấp nội dung dựa trên thiết bị (**Device based content delivery**)
    + Phản hồi nhanh hơn mà không cần **origin**: Giới hạn tốc độ (**Rate limiting**) / **API Mocking** / Kiểm tra tình trạng (**Health check**) / Xử lý Lỗi (**Error Handling**)

#### Các trường hợp sử dụng CloudFront

- Tài nguyên web tĩnh: Tỷ lệ truy cập bộ nhớ đệm cao => Tăng hiệu suất + Tải **origin** tối thiểu
- Phân phối toàn bộ trang web: Hiệu suất toàn cầu + Bảo mật + Tính khả dụng cao + Tối ưu hóa chi phí
- Tăng tốc **API** (**API acceleration**): Tái sử dụng kết nối + **AWS backbone** => Giảm độ trễ + Bộ nhớ đệm chọn lọc
- Truyền phát đa phương tiện (**Media Streaming**): Khả năng mở rộng không giới hạn + Độ trễ thấp + Hiệu quả về chi phí + Hàng triệu người dùng đồng thời
- Tải xuống tệp lớn: Yêu cầu phạm vi (**Range requests**) + Bộ nhớ đệm tại biên (**Edge caching**) + Tiết kiệm chi phí 80-90%

#### Các phương pháp hay nhất của CloudFront

- Khả năng hiển thị từ đầu đến cuối (**End to end visibility**): Giám sát người dùng thật/Internet/Hạ tầng
- Tối đa hóa bộ nhớ đệm: Chuẩn hóa khóa bộ nhớ đệm (**Normalize cache key**), bộ nhớ đệm nội dung động/riêng tư và bộ nhớ đệm lỗi
- Chặn các yêu cầu không mong muốn: Mô hình độc hại, giới hạn tốc độ, giảm thiểu **DDoS** và lọc các yêu cầu **API**
- Chuyển logic nghiệp vụ: Phản hồi **CORS**, chuyển hướng tại biên và đặt **cookies**
- **Failover** tự động: Định tuyến **failover** của **Route 53** + **CloudFront Origin Group** cho **failover** cấp yêu cầu

### AWS WAF & Bảo vệ Ứng dụng

#### Các mối đe dọa và tác động đến doanh nghiệp

- Từ chối dịch vụ (**Denial of Service**)
- Lỗ hổng Ứng dụng (**App Vulnerabilities**)
- Lưu lượng truy cập **Bot**

**=>** Dữ liệu bị đánh cắp, thông tin xác thực bị xâm phạm, spam, thời gian ngừng hoạt động, can thiệp thủ công, tăng chi phí hạ tầng, mất uy tín

#### Sự gia tăng hoạt động của bot

Các bot **AI** đa dạng từ nhiều nguồn khác nhau tăng đáng kể trong môi trường khách hàng: Tăng 155% so với cùng kỳ năm trước

#### Route 53 trong bảo vệ vòng ngoài

Các máy chủ **DNS** phân tán trên toàn cầu => Tự động mở rộng quy mô và giảm thiểu **DDoS** + **SLA** về Tính khả dụng 100%

#### Bảo vệ Hạ tầng với AWS Shield

- Tại biên (**At edge**):
    + **SYN Proxy**
    + Kiểm tra (**Inspection**)
    + Xác thực gói tin (**Packet validation**)
    + Dung lượng lọc phân tán (**Distributed scrubbing capacity**)
    + Chính sách Định tuyến Tự động

- Tại biên giới (**At border**):
    + Lọc Lưu lượng truy cập
    + Phát hiện và giảm thiểu cấp tài nguyên
    + Phát hiện dựa trên Tình trạng (**Health-based detection**)

#### Kiểm tra HTTP với AWS WAF

- **WAF** hoạt động cùng với **CloudFront**
- Phát hiện **HTTP flood**, các mẫu độc hại, **IP** xấu
- Kiểm soát **Bot**
- Kiểm soát Gian lận (**Fraud control**)

#### Phản hồi Sự cố của Shield Advanced

- **Shield/Shield Advanced** có **metric** để kích hoạt báo động nhằm bắt đầu phản hồi sự cố
- **Shield Advanced** cung cấp Đội ngũ Phản ứng **Shield (Shield Response Team)** 24/7
- Tìm kiếm vector tấn công và bên đóng góp

#### Cấu hình WAF

- Thêm quy tắc (**rules**) và nhóm quy tắc (**rules group**)
- Sử dụng quy tắc được quản lý (**managed rules**) và quy tắc tùy chỉnh (**custom rules**)
- Bắt đầu với chế độ **COUNT**: Giám sát để tránh dương tính giả (**false positives**)
- Sử dụng **labels** để tùy chỉnh logic và phản hồi
- Giới hạn phạm vi (**Scope-down**) để tối ưu hóa chi phí

#### Web ACL/Protection Pack

- Tập hợp các quy tắc, nhóm quy tắc và hành động mặc định
- Liên kết với các tài nguyên như **CloudFront Distribution**
- Cấu hình ghi nhật ký (**Logging**) và lấy mẫu (**sampling**)

#### WAF Rules

- Tiêu chí kiểm tra (**Inspection criteria**) (Địa chỉ **IP/Header value/Request Body**) => Hành động (**Action**) (Cho phép/Chặn/Đếm/Tùy chỉnh)
- Quy tắc dựa trên tỷ lệ (**Rate-based rule**): Dựa trên số lượng yêu cầu **HTTP** từ một **IP**

#### WAF Anti-DDoS Application Layer Protection

- Giảm thiểu **DDoS** lớp ứng dụng tự động
- Bảo vệ bằng các quy tắc được cấu hình sẵn
- Bảo vệ **DDoS** có thể cấu hình dựa trên nhu cầu ứng dụng

#### WAF Labels

Được thêm bởi quy tắc được quản lý (Luôn luôn) và quy tắc tùy chỉnh (Tùy chọn): Cho biết quy tắc đã khớp, trạng thái phiên, dữ liệu Địa lý và dựa trên **IP** cùng các hoạt động của **Bot/Fraud**

#### Common bots (Bot phổ biến)

Là gì: Các **bot** tự nhận dạng hoặc Công cụ Tìm kiếm, Mạng xã hội, thư viện **HTTP**
Kiểm soát **bot** phổ biến: Tìm kiếm yêu cầu, **IP**, **TLS fingerprint** và xác minh **bot** bằng **labels**

#### Evasive bots (Bot lẩn tránh)

Là gì: **Scrapers**, kẻ nhồi thông tin xác thực (**credential stuffers**), máy quét lỗ hổng, sử dụng các **header** và giá trị **browser** hiện có, không nổi tiếng và bắt chước các **bot** phổ biến thực
Giải pháp: Thẩm vấn máy khách (**Client interrogation**), xác định phiên máy khách duy nhất và giám sát hành vi của nó

### Thực hành CloudFront: Workshop Bảo vệ Vòng ngoài

#### So sánh giữa Origin lưu trữ S3 tĩnh thông thường và Origin lưu trữ S3 với CloudFront

- **CloudFront** phân phối một đối tượng nhanh hơn khi nó được phục vụ từ bộ nhớ đệm
- **CloudFront** phân phối một đối tượng nhanh hơn bằng cách sử dụng mạng toàn cầu **AWS** thay vì internet công cộng
- Trong **CloudFront**, nén được áp dụng, kích thước đối tượng được giảm đáng kể

#### Thực hành AWS WAF: Tăng cường Khả năng Phòng thủ Ứng dụng Web của Bạn với AWS WAF

- Đã tạo các quy tắc **WAF** để bảo vệ chống lại:
  + **Cross site Scripting (XSS)**
  + **SQL Injection**
  + Tấn công **Path traversal**
  + Tài sản phía máy chủ (**Server-side asset**)
  + **Bot** Phổ biến/Lẩn tránh (**Common/Evasive Bot**)
  + Lạm dụng **API** (**API misuse**)
  + Thử nghiệm Bí ẩn (**Mystery Test**): Cấu hình quy tắc để chặn quyền truy cập bằng giá trị **header** được mã hóa

### Trải nghiệm Sự kiện

Sự kiện này rất nhiều thông tin và đúng lúc vì nhóm chúng tôi vừa thiết lập **CloudFront** vào đêm hôm trước. Hệ thống định giá mới tốt hơn rất nhiều đối với chúng tôi, với **WAF** bổ sung để bảo mật

Workshop rất tương tác và vui vẻ, với **Mr.Julian** cũng hỗ trợ hết mức có thể
Chúng tôi cũng nhận được câu trả lời cho các câu hỏi của mình từ **Mr.Julian** sau sự kiện:

**Hỏi:** **WAF** có thể được sử dụng để kiểm soát và chặn hoạt động của **bot**, nhưng gần đây với sự cải tiến của **AI**: Đặc biệt là **Agent** mới của **Gemini**, có thể tương tác trực tiếp trên trình duyệt của chúng tôi, không phải **headless**, liệu **WAF** có thể giúp bảo vệ chống lại những thứ đó không?

**Đáp:** Với **Gemini**, các tương tác vẫn được gọi bằng **API** nên **WAF** có thể phát hiện điều đó, nhưng có một loại khác mà chúng ta không thể phát hiện, đó là **Claude Agent**, nhưng nó chỉ là một công cụ hỗ trợ tiếp cận và không gây ra nhiều hành động độc hại như **bot** thông thường, vì vậy trong tương lai gần chúng ta chưa cần phải lo lắng về nó.

#### Một số hình ảnh sự kiện
![Attendee](/images/4-Event/Event5AttendeePic.jpg)
_Hình ảnh Nhóm Người tham dự Sự kiện_

![Top24Kahoot](/images/4-Event/Event5KahootTop24.jpg)
_Đạt Top 24 trong Bài Quiz Kahoot cuối sự kiện_

![Full Threat Blocked](/images/4-Event/FullThreatBlockedWAFWorkshop.jpg)
_Đã chặn tất cả các mối đe dọa của workshop_