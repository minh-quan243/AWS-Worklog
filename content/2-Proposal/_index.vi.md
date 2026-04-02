---
title: "Proposal"
date: "2025-12-05"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

![AWS Logo](/images/2-Proposal/AWSLogo.png)

# Hệ thống Ứng phó Sự cố và Điều tra Số Tự động trên AWS

# Proposal Link: [Proposal](https://docs.google.com/document/d/1RcPJmiVxS80qdi0wOvHltBcSoZtRC97LngEZEkKC09k/edit?brid=hv-MPz2j8n_U1ZiCdslbyw&tab=t.0)

## 1. Tóm tắt 
Nhóm của chúng tôi đang xây dựng một giải pháp ứng phó sự cố và điều tra số tự động như một phần của chương trình thực tập AWS First Cloud Journey. Ý tưởng rất đơn giản—khi một vấn đề bảo mật xảy ra trong AWS, chúng tôi muốn hệ thống phản ứng tự động mà không cần chờ đợi sự can thiệp thủ công.

Chúng tôi đang tạo ra một nền tảng tự động phát hiện các phát hiện bảo mật từ GuardDuty, cô lập các tài nguyên bị ảnh hưởng, thu thập bằng chứng pháp y thông qua việc thu thập dữ liệu toàn diện, và cung cấp các phân tích và bảng điều khiển để các đội bảo mật có thể điều tra những gì đã xảy ra. Mọi thứ được xây dựng bằng Infrastructure-as-Code với AWS CDK, vì vậy khách hàng có thể dễ dàng triển khai nó vào tài khoản AWS của riêng họ.

## 2. Báo cáo Vấn đề
### Vấn đề là gì?
Tần suất và sự tinh vi ngày càng tăng của các mối đe dọa mạng đặt ra những rủi ro đáng kể cho các tổ chức dựa vào cơ sở hạ tầng đám mây. Quy trình ứng phó sự cố thủ công thường chậm chạp, không nhất quán và dễ mắc lỗi của con người, có thể dẫn đến thời gian ngừng hoạt động hệ thống kéo dài, vi phạm dữ liệu và tổn thất tài chính. Dự án nhằm giải quyết những thách thức này bằng cách phát triển một hệ thống ứng phó sự cố tự động, đáng tin cậy và có khả năng mở rộng.

### Giải pháp
Các trường hợp sử dụng chính bao gồm phát hiện việc sử dụng trái phép thông tin xác thực AWS, xác định các EC2 instance bị xâm nhập, và đảm bảo dữ liệu pháp y được thu thập, xử lý và lưu trữ đúng cách để điều tra. Kiến trúc của chúng tôi tích hợp VPC Flow Logs, CloudTrail, CloudWatch và GuardDuty để phát hiện các mối đe dọa, trong khi Step Functions điều phối quy trình ứng phó tự động bao gồm cô lập EC2, tách khỏi ASG, tạo Snapshot và cách ly IAM. Tất cả bằng chứng được thu thập và xử lý thông qua Lambda ETL tùy chỉnh và Data Firehose, sử dụng Athena để phân tích pháp y. Hệ thống cũng bao gồm alert dispatching, notification bằng email và Slack, và cho chúng ta cái dashboard để analyze và điều tra chuyện đã xảy ra.

### Lợi ích và Tỷ suất Lợi nhuận (ROI)
* **Phát hiện mối đe dọa nhanh chóng**: Phản ứng tự động giúp giảm thiểu khoảng thời gian dễ bị tổn thương.
* **Thu thập bằng chứng toàn diện**: Tự động hóa việc thu thập dữ liệu pháp y, tạo điều kiện cho các cuộc điều tra nhanh hơn.
* **Hiệu quả về chi phí**: Tận dụng các dịch vụ serverless của AWS giúp giảm thiểu chi phí cơ sở hạ tầng.
* **Cải thiện tư thế bảo mật**: Thông qua giám sát liên tục và cảnh báo thời gian thực.
* **Thông tin chi tiết hữu ích**: Các bảng điều khiển và phân tích trao quyền cho các đội bảo mật.
* **Khả năng mở rộng**: Thích ứng với các tổ chức có quy mô và khối lượng sự cố khác nhau.

## 3. Kiến trúc Giải pháp
Giải pháp của chúng tôi sử dụng một kiến trúc đa giai đoạn toàn diện cho việc ứng phó sự cố và điều tra số tự động:

![AWS Architecture](/images/2-Proposal/AWSWorkshopArchitecture-Final.png)

### Các Dịch vụ AWS Được Sử dụng
- **Amazon GuardDuty**: Liên tục theo dõi các mối đe dọa bảo mật và hoạt động đáng ngờ.
- **AWS Step Functions**: Điều phối quy trình ứng phó sự cố.
- **AWS Lambda**: Chạy mã tự động hóa để cô lập và xử lý dữ liệu.
- **Amazon EventBridge**: Định tuyến các phát hiện từ GuardDuty đến Step Functions.
- **Amazon S3**: Lưu trữ bằng chứng pháp y và lưu trữ bảng điều khiển tĩnh.
- **Amazon Athena**: Cho phép thực hiện các truy vấn SQL trên các tập dữ liệu pháp y.
- **Amazon API Gateway**: Tạo điều kiện giao tiếp giữa bảng điều khiển và backend.
- **Amazon Cognito**: Bảo mật quyền truy cập cho người dùng bảng điều khiển.
- **Amazon CloudFront**: Tăng tốc độ phân phối bảng điều khiển trên toàn cầu.
- **Amazon SNS & SES**: Xử lý thông báo qua tin nhắn và email.
- **AWS CloudTrail**: Ghi nhật ký tất cả các hành động để kiểm toán.
- **Amazon CloudWatch**: Giám sát và bảng điều khiển.
- **Amazon EC2**: Các instance tùy chọn để phân tích.
- **AWS KMS**: Quản lý khóa để mã hóa.
- **Amazon Kinesis Data Firehose**: Truyền dữ liệu đến S3.

### Thiết kế Thành phần
- **Lớp Thu thập Dữ liệu & Phát hiện**: Thu thập các sự kiện từ VPC Flow Logs, CloudTrail, CloudWatch, EC2 và GuardDuty.
- **Lớp Xử lý Sự kiện**: Alert Dispatch, EventBridge định tuyến các phát hiện đến Step Functions; các sự kiện được phân loại theo loại.
- **Điều phối Ứng phó Tự động (Orchestration)**: Step Functions xử lý phân tích, ra quyết định, cô lập EC2, bảo vệ chấm dứt, tách ASG, tạo snapshot và cách ly IAM.
- **Lớp Xử lý Dữ liệu & Phân tích**: ETL pipeline với Lambda và Data Firehose xử lý nhật ký thô vào S3; Athena truy vấn dữ liệu.
- **Lớp Bảng điều khiển & Phân tích**: Bảng điều khiển React lưu trữ trên S3 với xác thực Cognito, sử dụng dữ liệu qua API Gateway và Athena.

## 4. Triển khai Kỹ thuật
### Các Giai đoạn Triển khai
Chúng tôi sử dụng Agile Scrum với các sprint 1 tuần trong vòng 6 tuần:
- **Sprint 1**: Nền tảng & Thiết lập (VPC, Security Groups, Đào tạo).
- **Sprint 2**: Điều phối Cốt lõi (Step Functions, Lambda, tích hợp GuardDuty).
- **Sprint 3**: Dữ liệu & Phân tích (S3, Athena, ETL pipeline).
- **Sprint 4**: Bảng điều khiển & UI (Trang web tĩnh, API Gateway, CloudFront).
- **Sprint 5**: Kiểm thử & Tối ưu hóa (Cognito, Kiểm thử hiệu suất, Mô phỏng).
- **Sprint 6**: Tài liệu & Bàn giao (Hướng dẫn, Demo, Hoàn thiện).

### Yêu cầu Kỹ thuật
- **Frontend & Bảng điều khiển**: HTML/CSS/JS tùy chỉnh được lưu trữ trên S3, phục vụ qua CloudFront.
- **Backend & Xử lý**: Python 3.12 cho Lambda, Step Functions để điều phối.
- **Dữ liệu & Lưu trữ**: S3 cho bằng chứng, Athena để truy vấn, Firehose để truyền dữ liệu.
- **Cơ sở hạ tầng**: Tất cả được định nghĩa trong AWS CDK (Python).
- **Bảo mật**: GuardDuty để phát hiện, IAM cho quyền hạn tối thiểu, KMS để mã hóa.

## 5. Thời gian biểu & Cột mốc
**Dòng thời gian Dự án**
**Dòng thời gian Dự án**
- **Tuần 6-7 (Nền tảng & Thiết lập)**
    - **Hoạt động**: Đào tạo nhóm về GuardDuty/Step Functions, đánh giá thiết kế kiến trúc, thiết lập VPC và bảo mật.
    - **Sản phẩm bàn giao**: Tài liệu kiến trúc v1, hoàn thành đào tạo nhóm, kho lưu trữ GitHub được thiết lập.
- **Tuần 7-9 (Điều phối Cốt lõi)**
    - **Hoạt động**: Phát triển quy trình Step Functions, lập trình hàm Lambda cho tất cả các hành động ứng phó, tích hợp EventBridge, thiết lập SNS/SES, kiểm thử tích hợp.
    - **Sản phẩm bàn giao**: Định nghĩa máy trạng thái Step Functions, hơn 7 hàm Lambda có tài liệu, tích hợp GuardDuty, hệ thống thông báo, API Gateway.
- **Tuần 10 (Dữ liệu & Phân tích)**
    - **Hoạt động**: Thiết lập lưu trữ pháp y S3, tạo bảng Athena, phát triển đường ống ETL, thư viện truy vấn SQL.
    - **Sản phẩm bàn giao**: Hơn 15 truy vấn Athena được ghi lại, sách hướng dẫn phân tích pháp y, lưu trữ dữ liệu đã xử lý.
- **Tuần 11 (Bảng điều khiển & UI)**
    - **Hoạt động**: Phát triển bảng điều khiển tĩnh, xác thực Cognito, thiết lập API Gateway, cấu hình CloudFront CDN, tích hợp bảng điều khiển.
    - **Sản phẩm bàn giao**: Bảng điều khiển lưu trữ trên S3, hệ thống xác thực, giao diện truy vấn, tích hợp kết quả thời gian thực.
- **Tuần 12 (Kiểm thử, Xác thực & Tối ưu hóa)**
    - **Hoạt động**: Kiểm thử thủ công, quét bảo mật bao gồm các kịch bản sự cố mô phỏng (hơn 5 quy trình), kiểm thử hiệu suất, mô phỏng tấn công. Tối ưu hóa dữ liệu với truy vấn Athena và Data Firehose.
    - **Sản phẩm bàn giao**: Kết quả quét bảo mật, video mô phỏng sự cố, tối ưu hóa dữ liệu.
- **Tuần 13 (Tài liệu & Bàn giao)**
    - **Hoạt động**: Hướng dẫn triển khai, tài liệu API, các phiên chuyển giao kiến thức, demo cuối cùng, dọn dẹp GitHub.
    - **Sản phẩm bàn giao**: Kho lưu trữ GitHub hoàn chỉnh (công khai), hướng dẫn triển khai, buổi trình diễn workshop trực tiếp.

## 6. Ước tính Ngân sách
Bạn có thể tìm thấy ước tính ngân sách chi tiết trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=b9b2c0423dcd3b21dadd62e5053a5fdf2d003339).

### Chi phí Cơ sở hạ tầng
Chi phí triển khai hàng tháng điển hình (Bậc miễn phí / Quy mô nhỏ): **~$5.01**

- **GuardDuty**: ~$1.80/tháng
- **S3**: ~$1.07/tháng
- **KMS**: ~$1.12/tháng
- **CloudTrail**: ~$0.55/tháng
- **Athena**: ~$0.29/tháng
- **Amazon Simple Email Service (SES)**: ~$0.09/tháng
- **Amazon API Gateway**: ~$0.05/tháng
- **Amazon Data firehose**: ~$0.04/tháng
- **Lambda, Step Functions, SNS**: Thường nằm trong giới hạn Bậc miễn phí cho mức sử dụng thông thường.

**Lưu ý**: Chi phí giả định mức sử dụng thông thường từ 20-150 sự cố mỗi tháng.

## 7. Đánh giá Rủi ro
### Ma trận Rủi ro
- **Tắc nghẽn Hiệu suất**: Khối lượng dữ liệu lớn làm chậm các truy vấn.
- **Vi phạm Bảo mật**: Xâm phạm chính dữ liệu pháp y.
- **Vượt quá Chi phí**: Ghi nhật ký không kiểm soát hoặc vòng lặp vô hạn.

### Chiến lược Giảm thiểu
- **Hiệu suất**: Giám sát Athena/Firehose; tối ưu hóa truy vấn; điều chỉnh tài nguyên động.
- **Bảo mật**: Mã hóa (KMS), vai trò IAM nghiêm ngặt, ghi nhật ký kiểm toán.
- **Chi phí**: Cảnh báo ngân sách AWS, phát hiện bất thường chi phí, giới hạn tự động mở rộng.
- **Khôi phục sau Thảm họa**: Sao lưu, quy trình chuyển đổi dự phòng và các biện pháp dự phòng.

## 8. Kết quả Dự kiến
### Cải tiến Kỹ thuật
- **Ứng phó Tự động**: Cô lập không chạm (zero-touch) các tài nguyên bị xâm nhập.
- **Tốc độ**: Giảm thời gian điều tra từ hàng giờ xuống còn vài phút.
- **Độ tin cậy**: Thu thập bằng chứng nhất quán, có thể lặp lại mà không có lỗi của con người.

### Giá trị Dài hạn
- **Kiến trúc Có thể Mở rộng**: Nền tảng cho tự động hóa bảo mật trong tương lai.
- **Kiến thức**: Năng lực của nhóm về bảo mật AWS nâng cao và các khái niệm serverless.
- **Tài sản Có thể Tái sử dụng**: Một giải pháp có thể triển khai cho các khách hàng hoặc nhóm AWS khác.

---
**Trạng thái**: Sẵn sàng cho Xem xét & Phê duyệt
**Mã Dự án**: AWS-FCJ-IR-FORENSICS-2025