---
title: "Blog 2"
date: "2000-10-03"
weight: 02
chapter: false
pre: " <b> 3.2. </b> "
---
## [AWS DevOps & Developer Productivity Blog](https://aws.amazon.com/blogs/devops/)

# Amazon Q Developer CLI hỗ trợ đầu vào hình ảnh trong terminal của bạn

bởi Keerthi Sreenivas Konjety vào ngày 21 tháng 5 2025 trong [Amazon Q Developer](https://aws.amazon.com/blogs/devops/category/amazon-q/amazon-q-developer/), [Announcements](https://aws.amazon.com/blogs/devops/category/post-types/announcements/) | [Permalink](https://aws.amazon.com/blogs/devops/amazon-q-developer-cli-supports-image-inputs-in-your-terminal/) | Chia sẻ

Trong bài đăng này, tôi sẽ khám phá cách tính năng hỗ trợ hình ảnh trong [Amazon Q Developer Command Line Interface (CLI)](https://aws.amazon.com/q/developer/) thay đổi quy trình làm việc phát triển. Q Developer CLI gần đây đã bổ sung hỗ trợ hình ảnh, mở rộng khả năng xử lý thông tin trực quan và tăng cường năng suất của nhà phát triển. Tính năng mới này cho phép các nhà phát triển tương tác trực tiếp với sơ đồ, bản thiết kế kiến trúc và các tài sản trực quan khác thông qua dòng lệnh.

Phát triển phần mềm hiện đại ngày càng dựa vào các biểu diễn trực quan để truyền đạt ý tưởng. Ví dụ, sơ đồ kiến trúc minh họa các thành phần hệ thống và sự tương tác của chúng, trong khi sơ đồ thực thể liên kết phác thảo cấu trúc cơ sở dữ liệu. Việc chuyển đổi tài sản trực quan thành mã hoạt động thường là một quy trình giải thích và triển khai thủ công, dễ xảy ra lỗi.

Tính năng hỗ trợ hình ảnh mới trong Q Developer CLI thu hẹp khoảng cách này bằng cách cho phép các nhà phát triển cung cấp hình ảnh trực tiếp cho tác nhân Q Developer CLI để phân tích. Tôi rất hào hứng khi sử dụng tính năng này để chuyển đổi các sơ đồ kiến trúc của mình từ các ý tưởng phác thảo, vẽ tay thành các tài liệu thiết kế trau chuốt, và sau đó thành cơ sở hạ tầng dưới dạng mã. Tôi mong muốn áp dụng nó trong nhiều trường hợp sử dụng khác nhau, cho dù tôi đang bắt đầu một dự án mới hay tinh giản các quy trình làm việc hàng ngày của mình.

Tại thời điểm ra mắt, Q Developer CLI hỗ trợ các định dạng hình ảnh JPEG, PNG, WEBP và GIF, cùng với khả năng tải lên 10 hình ảnh cho mỗi yêu cầu. Bạn phải sử dụng phiên bản mới nhất (1.10.0 trở lên) của Q Developer CLI để tận hưởng tính năng hỗ trợ hình ảnh trong Q Developer CLI. Hãy sử dụng [hướng dẫn](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html) này để nâng cấp hoặc cài đặt phiên bản mới nhất.

Tôi sẽ sử dụng bốn tình huống sau làm ví dụ để chứng minh lợi ích của hỗ trợ hình ảnh cho Q Developer CLI.

### **Trường hợp sử dụng 1: Tạo cơ sở hạ tầng dưới dạng mã từ sơ đồ kiến trúc**

Sơ đồ sau mô tả một ứng dụng thay đổi kích thước hình ảnh. Nó bao gồm một bucket [Amazon S3](https://aws.amazon.com/s3/) nguồn mà người dùng tải hình ảnh lên, và một hàm [AWS Lambda](https://aws.amazon.com/lambda/) thay đổi kích thước hình ảnh và lưu trữ nó trong một S3 Bucket đích. Giờ đây tôi có thể chuyển đổi sơ đồ kiến trúc thành mã bằng Q Developer CLI.

![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/architecture-chart-1.png)
_Kiến trúc cho một ứng dụng thay đổi kích thước hình ảnh_

Trong ảnh chụp màn hình sau, tôi đã yêu cầu Q Developer CLI: “Vui lòng cung cấp cho tôi một mẫu terraform tham chiếu sử dụng các phương pháp hay nhất”. Lưu ý rằng việc kéo và thả hình ảnh vào CLI sẽ thêm đường dẫn vào lời nhắc của bạn.
![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/usecase1-sc1.jpg)
_CLI với mã Terraform được tạo bởi Q Developer_

Hình ảnh trên cho thấy một phần phản hồi mà Q Developer CLI đã tạo ra.

Q Developer phản hồi bằng mẫu terraform cần thiết để bắt đầu xây dựng ứng dụng thay đổi kích thước hình ảnh. Q Developer CLI đã phân tích hình ảnh, xác định các thành phần và mối quan hệ của chúng, rồi tạo mã Terraform tương ứng. Mặc dù không hiển thị trong hình ảnh, phản hồi bao gồm mã của hàm Lambda bằng Python và quyền IAM cần thiết cho hàm Lambda.

Trước đây, việc chuyển đổi sơ đồ này thành cơ sở hạ tầng dưới dạng mã sẽ yêu cầu tôi phải tự giải thích thủ công từng thành phần và viết cấu hình tương ứng. Với hỗ trợ hình ảnh, giờ đây tôi có thể tự động hóa phần lớn quy trình này và tinh chỉnh mã được tạo thông qua một cuộc trò chuyện với Q Developer. Sau đó, tôi có thể trò chuyện với Q Developer để tinh chỉnh mã đã tạo, đặt câu hỏi về các chi tiết triển khai cụ thể hoặc yêu cầu sửa đổi dựa trên các yêu cầu bổ sung và xuất mã sang tệp .tf.

### **Trường hợp sử dụng 2: Chuyển đổi sơ đồ ER thành lược đồ cơ sở dữ liệu**

Đối với tình huống thứ hai, hãy xem xét một trường hợp sử dụng trong đó tôi là một phần của nhóm mô hình hóa dữ liệu đang phát triển phần mềm quản lý khóa học cho các trường đại học. Tôi đã tạo một sơ đồ thực thể liên kết (ER) cho các cấu trúc dữ liệu cốt lõi của họ. Giờ đây tôi có thể sử dụng Q Developer để giúp tôi chuyển đổi sơ đồ ER thành SQL.

![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/ER-Diagram.png)
_Sơ đồ thực thể liên kết cho hệ thống Quản lý Khóa học_

Trong ảnh chụp màn hình sau, tôi đã yêu cầu Q Developer CLI sử dụng sơ đồ ER để tạo lược đồ cơ sở dữ liệu.

![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/Usecase2-sc1.jpg)
_CLI với câu lệnh của người dùng và mã SQL được tạo bởi Q Developer_
![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/usecase-2-sc2.jpg)
_CLI với mã SQL được tạo bởi Q Developer_

Hình ảnh trên cho thấy phản hồi mà Q Developer CLI đã tạo ra.

Q Developer đã phân tích sơ đồ, xác định các thực thể, thuộc tính và mối quan hệ, sau đó tạo mã SQL thích hợp để tạo lược đồ cơ sở dữ liệu.

Sau khi Q Developer đưa ra kết quả, tôi có thể tinh chỉnh lược đồ này thông qua một cuộc trò chuyện với Q Developer bằng cách yêu cầu thay đổi độ dài chuỗi, chỉ mục, v.v., hoặc yêu cầu giải thích về các quyết định thiết kế.

### **Trường hợp sử dụng 3: Chuyển đổi hình ảnh vẽ tay thành tài liệu thiết kế**

Hãy xem xét một tình huống trong đó tôi đã động não ra một ý tưởng trên giấy và tôi muốn chia sẻ ý tưởng này với nhóm của mình. Trong hình ảnh sau, tôi đã vẽ tay quy trình đặt hàng cho một trang web. Khi người dùng trang web đặt mua sách từ trang web, ứng dụng sẽ cập nhật kho hàng, sau đó gọi các hành động thanh toán và giao hàng. Giờ đây tôi có thể sử dụng Q Developer CLI để phác thảo tài liệu từ ý tưởng vẽ tay.

![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/hand-drawn-diagram-scaled.jpg)
_Sơ đồ vẽ tay của luồng quy trình đặt hàng cho một trang web_

Trong ví dụ sau, tôi đã yêu cầu Q Developer viết một tài liệu thiết kế bằng cách sử dụng hình ảnh này làm tham chiếu.
![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/usecase3-sc1.jpg)
_CLI với câu lệnh của người dùng và phản hồi được tạo bởi Q Developer_

Ảnh chụp màn hình trên cho thấy, Q Developer trước tiên đã đọc hình ảnh và hiểu nội dung từ sơ đồ vẽ tay.
![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/usecase3-sc2.jpg)
_CLI với phản hồi được tạo bởi Q Developer_

Ảnh chụp màn hình trên là một phần phản hồi mà Q Developer CLI đã tạo ra.

Q Developer đã chuyển đổi ý tưởng thành một tài liệu thiết kế bao gồm kiến trúc hệ thống, luồng xử lý, mô hình dữ liệu, các yêu cầu chức năng, và các yêu cầu kỹ thuật. Tôi cũng có thể yêu cầu Q Developer xuất toàn bộ nội dung sang tệp .md. Điều này giảm lượng thời gian từ ý tưởng đến thực thi và tinh giản việc viết tài liệu.

### **Trường hợp sử dụng 4: Xây dựng bản mô phỏng UI/wireframe từ ảnh chụp màn hình**

Giả sử tôi muốn bắt đầu xây dựng Giao diện Người dùng (UI) từ tài liệu thiết kế của mình trong trường hợp sử dụng 3. Tôi có thể cung cấp một hình ảnh tham chiếu cho Q Developer để tạo các wireframe ban đầu cho UI của mình.

![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/book_website.png)
_Trang chủ mẫu của trang web bán sách_

Trong ví dụ này, tôi đã yêu cầu Q Developer giúp tạo front-end cho một trang web mới bằng Vue.js
![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/usecase4-sc1.jpg)
_CLI với câu lệnh của người dùng và phản hồi được tạo bởi Q Developer_
![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/usecase4-sc4.jpg)
_CLI với mã Vue.js được tạo bởi Q Developer_

Hình ảnh trên cho thấy một phần mã Vue.js được tạo bởi Q Developer CLI để tái tạo front-end của trang web trong ảnh chụp màn hình. Sau khi tôi xác minh mã, tôi có thể yêu cầu Q Developer CLI tạo các tệp này cục bộ.

Cách tiếp cận này giảm các khía cạnh dễ xảy ra lỗi của việc tạo wireframe, cho phép tôi tập trung vào các quyết định thiết kế sáng tạo thay vì các tác vụ thiết lập lặp đi lặp lại. Bằng cách này, tôi có thể tăng tốc chu kỳ phát triển, đảm bảo tính nhất quán giữa các thành phần và cung cấp một nền tảng có thể dễ dàng tùy chỉnh để đáp ứng các yêu cầu dự án cụ thể.

### **Các khả năng bổ sung:**

Ngoài các ví dụ trên, Q Developer CLI có thể phân tích nhiều loại hình ảnh, bao gồm:

  * Sơ đồ luồng (Flow charts) và sơ đồ quy trình
  * Sơ đồ lớp (Class diagrams) cho thiết kế hướng đối tượng
  * Sơ đồ cấu trúc liên kết mạng (Network topology diagrams)
  * Ảnh chụp màn hình của thông báo lỗi hoặc trạng thái ứng dụng

Tính linh hoạt này làm cho Q Developer CLI trở thành một công cụ mạnh mẽ cho các quy trình làm việc phát triển khác nhau.

**Kết luận:**

Việc bổ sung hỗ trợ hình ảnh cho Amazon Q Developer CLI đại diện cho một bước tiến đáng kể trong việc thu hẹp khoảng cách giữa các biểu diễn trực quan và văn bản trong phát triển phần mềm. Bằng cách cho phép tôi làm việc với sơ đồ và các tài sản trực quan khác trực tiếp từ dòng lệnh, Amazon Q Developer cải thiện hiệu suất của tôi trong việc chuyển đổi thiết kế thành triển khai, giảm lỗi và tăng tốc các chu kỳ phát triển. Tôi khuyến khích bạn khám phá khả năng mới này và tìm hiểu cách nó có thể tăng cường quy trình làm việc phát triển của bạn.

Để tìm hiểu thêm về Q Developer và các khả năng của nó, hãy truy cập [tài liệu](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/what-is.html).

#### **Giới thiệu về Tác giả:**

![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2025/05/20/keerthi_photo-150x150.jpg)
**Keerthi Sreenivas Konjety**

[Keerthi Sreenivas Konjety](https://www.linkedin.com/in/keer718/) là Specialist Solutions Architect cho Amazon Q Developer, với hơn 3,5 năm kinh nghiệm về AI, ML và Kỹ thuật Dữ liệu. Chuyên môn của cô là tăng cường năng suất của nhà phát triển cho khách hàng AWS. Ngoài công việc, cô yêu thích nhiếp ảnh và [sáng tạo nội dung AI](https://www.instagram.com/qriositybykeerthi/).

TAGS: [Developer Tools](https://aws.amazon.com/blogs/devops/tag/developer-tools/), [Development](https://aws.amazon.com/blogs/devops/tag/development/)
