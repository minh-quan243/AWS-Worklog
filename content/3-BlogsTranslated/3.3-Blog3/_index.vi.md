---
title: "Blog 3"
date: "2000-10-04"
weight: 03
chapter: false
pre: " <b> 3.3. </b> "
---
## [Blog AWS Cloud Operations](https://aws.amazon.com/blogs/mt/)

# Mô phỏng lỗi cục bộ với AWS Fault Injection Service

bởi Ozgur Canibeyaz và Pablo Colazurdo vào ngày 30 THÁNG 6 2025 trong [AWS Fault Injection Service (FIS)](https://aws.amazon.com/blogs/mt/category/developer-tools/aws-fault-injection-service-fis/"), [AWS Resilience Hub (ARH)](https://aws.amazon.com/blogs/mt/category/management-and-governance/aws-resilience-hub/), [AWS Systems Manager](https://aws.amazon.com/blogs/mt/category/management-tools/aws-systems-manager/), [Management Tools](https://aws.amazon.com/blogs/mt/category/management-tools/), [Resilience](https://aws.amazon.com/blogs/mt/category/resilience), [Technical How-to](https://aws.amazon.com/blogs/mt/category/post-types/technical-how-to/) |  [Permalink](https://aws.amazon.com/blogs/mt/simulating-partial-failures-with-aws-fault-injection-service/) | Chia sẻ

Các hệ thống phân tán hiện đại phải có khả năng chống chịu lỗi trước các gián đoạn không mong muốn để duy trì tính khả dụng, hiệu suất và độ ổn định. [Chaos engineering](https://en.wikipedia.org/wiki/Chaos_engineering) giúp các nhóm phát hiện những điểm yếu tiềm ẩn bằng cách thực hiện fault injection lên hệ thống và quan sát cách nó phục hồi. Trong khi thử nghiệm truyền thống xác thực hành vi mong đợi, chaos engineering kiểm tra khả năng phục hồi của hệ thống trong suốt thời gian xảy ra lỗi. [AWS Fault Injection Service](https://aws.amazon.com/fis/) (AWS FIS) là một dịch vụ AWS được quản lý toàn diện giúp các nhóm chạy các thí nghiệm fault injection trên các workloads của AWS. Nó hỗ trợ các tình huống như chấm dứt [Amazon EC2 instances](https://aws.amazon.com/ec2/), điều tiết các [Amazon API Gateway](https://aws.amazon.com/api-gateway/) requests, và tạo độ trễ mạng. Điều này cho phép bạn xác thực khả năng phục hồi trong các môi trường giống như môi trường sản xuất. Mặc dù những khả năng này rất mạnh mẽ, nhiều lỗi thực tế chỉ ảnh hưởng đến một phần lưu lượng truy cập.

Trong bài đăng này, bạn sẽ tìm hiểu cách mô phỏng lỗi cục bộ. Một kiểu lỗi phổ biến nhưng ít được kiểm tra—bằng cách kết hợp AWS FIS với weighted routing trong [Application Load Balancer (ALB)](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/) và một hàm [AWS Lambda](https://aws.amazon.com/lambda/) trả về các phản hồi lỗi tùy chỉnh. Phương pháp này cho phép bạn kiểm tra cách ứng dụng của bạn xử lý các điều kiện suy giảm mà không cần thay đổi mã hoặc làm gián đoạn luồng truy cập thông thường.

## **Tổng quan về giải pháp**

Giải pháp của chúng tôi kết hợp AWS FIS với định tuyến có trọng số của ALB để điều hướng một tỷ lệ phần trăm lưu lượng truy cập có thể định cấu hình đến một hàm Lambda có thể trả về các lỗi mô phỏng. Cách tiếp cận này không yêu cầu thay đổi mã nguồn ứng dụng và sẽ tự động khôi phục về hoạt động bình thường sau khi kiểm thử.
![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/15/Experiment_Flow-1.png)
  
_Hình 1 Giải pháp này cho thấy cách sửa đổi Load Balancer của bạn một cách an toàn để mô phỏng lỗi trong quá trình thực hiện thử nghiệm và khôi phục an toàn sau khi kết thúc_

## **Lợi ích chính**

Giải pháp này cung cấp các lợi ích chính sau cho các nhóm triển khai kỹ thuật hỗn loạn:

  * Mô phỏng lỗi có kiểm soát.
  * Không cần sửa đổi ứng dụng.
  * Thiết lập và khôi phục tự động.
  * Tỷ lệ lỗi có thể định cấu hình.



## **Hướng dẫn triển khai**

## **Điều kiện tiên quyết**

Trước khi bắt đầu, hãy xác minh bạn có:

  * Một tài khoản AWS với quyền triển khai các stack [AWS CloudFormation](https://aws.amazon.com/cloudformation/) và quản lý các thí nghiệm AWS FIS.
  * Một ALB hiện có được cấu hình với một target group định tuyến lưu lượng truy cập đến một microservice đang chạy.
  * ALB phải đã hoạt động và có thể truy cập công khai để kiểm tra các lỗi mô phỏng.
  * Truy cập vào [AWS Command Line Interface](https://aws.amazon.com/cli/)(AWS CLI) hoặc AWS Management Console.



## **Bước 1: Triển khai mẫu CloudFormation**

Mẫu CloudFormation thiết lập tất cả các tài nguyên cần thiết, bao gồm:

  * Một hàm Lambda để mô phỏng các phản hồi lỗi.
  * Một tài liệu [AWS Systems Manager (SSM)](https://aws.amazon.com/systems-manager/) automation.
  * Một IAM Role cấp quyền cho AWS FIS để gọi tài liệu SSM Automation.
  * Một [AWS FIS experiment template](https://docs.aws.amazon.com/fis/latest/userguide/experiment-template-example.html) được cấu hình sẵn.


![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/15/Overall_Architecture-2.png)
  
_Hình 2 Cái nhìn cấp cao về các thành phần giải pháp và sự tương tác của chúng._

## **Các tham số thí nghiệm có thể định cấu hình**

Mẫu CloudFormation yêu cầu ba tham số sau khi triển khai:

  * Tên Application Load Balancer.
  * ARN của Listener ALB rule cần sửa đổi.
  * Thời lượng kiểm tra bằng giây — lỗi cục bộ nên kéo dài bao lâu.



Các cài đặt thí nghiệm khác, chẳng hạn như tỷ lệ phần trăm lưu lượng truy cập cần chuyển hướng và mã phản hồi Lambda, được cấu hình sẵn trong định nghĩa thí nghiệm. Nếu bạn muốn tùy chỉnh các giá trị này, bạn có hai tùy chọn:

**Tùy chọn 1: Sửa đổi Mẫu CloudFormation và triển khai lại**

Bạn có thể chỉnh sửa trường `documentParameters` trong phần định nghĩa thí nghiệm của mẫu để thay đổi:

  * FailurePercentage (ví dụ: 10, 50, 100).



Để thay đổi HTTP status code được hàm Lambda trả về (ví dụ: từ 500 thành 404), hãy sửa đổi giá trị `statusCode` trực tiếp trong khối mã nội tuyến bên trong mẫu.

Sau khi chỉnh sửa, hãy triển khai lại stack để áp dụng các thay đổi của bạn.

**Tùy chọn 2: Tạo phiên bản mới của tài liệu SSM Automation**

Nếu bạn không muốn triển khai lại stack:

  1. Truy cập vào **AWS Systems Manager** **→** **Documents** console.
  2. Định vị tài liệu SSM được tạo bởi mẫu.
  3. Chọn **Create new version** và điều chỉnh các giá trị mặc định như FailurePercentage.
  4. Sử dụng phiên bản đã cập nhật bằng cách tham chiếu nó trong một thí nghiệm AWS FIS mới (thông qua CLI hoặc console).



**IAM Permissions:**

Bạn cần có quyền tạo các IAM role và policy khi triển khai CloudFormation template. Khi triển khai thông qua AWS Management Console, bạn sẽ cần xác nhận rằng template tạo ra các tài nguyên IAM. Nếu sử dụng AWS CLI, hãy thêm flag `--capabilities CAPABILITY_NAMED_IAM`.

**Tải xuống template:** Bạn có thể tải xuống [CloudFormation template tại đây](https://d2908q01vomqb2.cloudfront.net/artifacts/MTBlog/cloudops-1899/fis_template.yaml) và lưu cục bộ dưới dạng `fis_template.yaml` trước khi triển khai nó thông qua AWS Console hoặc CLI.
    
    
  ```bash
aws cloudformation create-stack --stack-name alb-fis-experiment \
    --template-body file://fis_template.yaml \
    --parameters \
        ParameterKey=LoadBalancerName,ParameterValue=LoadBalancerName \
        ParameterKey=ListenerRuleArn,ParameterValue=RuleARN \
        ParameterKey=TestDurationInSeconds,ParameterValue=60 \
    --capabilities CAPABILITY_NAMED_IAM
```
`LoadBalancerName` và `RuleARN` đề cập đến tên Load Balancer và ARN đầy đủ của Listener rule trước dịch vụ mà bạn muốn mô phỏng lỗi. 60 chỉ định thời lượng của lỗi mô phỏng bằng giây.

**Lưu ý:** `FISExperimentRole` IAM policy sử dụng `"Resource": "*"` cho các hành động nhất định để cho phép AWS FIS sửa đổi các tài nguyên Load Balancer được tạo động. Bởi vì tên tài nguyên như ARN của target group không được biết tại thời điểm triển khai, nên việc giới hạn phạm vi các quyền này là không khả thi trong ngữ cảnh của bài đăng này. Mặc dù điều này mang lại sự linh hoạt, [AWS security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) khuyến nghị giới hạn phạm vi quyền đối với các tài nguyên cụ thể bất cứ khi nào có thể. Nếu bạn biết chính xác các tài nguyên sẽ được sử dụng, hãy cân nhắc cập nhật chính sách để hạn chế quyền truy cập cho phù hợp.

## **Bước 2: Xác minh Hàm Lambda**

Sau khi triển khai, hãy kiểm tra hàm Lambda trong AWS console để xác nhận nó trả về phản hồi lỗi mong đợi. Hàm phải trả về nội dung như sau:
    
    
 ```json
{
  "statusCode": 503,
  "body": "Service Unavailable - Simulated Error Response"
}
```

## **Bước 3: Bắt đầu Thí nghiệm AWS FIS**

  1. Mở AWS Fault Injection Service console.
  2. Định vị template được cấu hình sẵn trong Experiment Templates.
  3. Chọn Start experiment.
  4. Xác nhận và khởi chạy kiểm tra.


![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/13/Fis_Console-2.png)

_Hình 3 AWS FIS console hiển thị experiment template tùy chỉnh được tạo bởi mẫu CloudFormation._

Khi bạn bắt đầu thí nghiệm, AWS FIS sẽ gọi một AWS Systems Manager Automation Document được tạo trong quá trình triển khai. Automation này thực hiện các hành động sau:

  1. **Tạo một ALB target group mới** trỏ đến một hàm Lambda được cấu hình để trả về các error responses mô phỏng.
  2. **Sửa đổi một ALB Listener rule** để chia một phần lưu lượng truy cập đến target group mới này, mô phỏng hiệu quả một lỗi cục bộ.
  3. **Đợi trong một khoảng thời gian xác định** (có thể định cấu hình thông qua CloudFormation template).
  4. **Khôi phục ALB listener rule** về trạng thái ban đầu và xóa target group tạm thời.



Toàn bộ vòng đời này được tự động hóa — bạn không cần viết bất kỳ mã nào hoặc thực hiện cập nhật thủ công nào cho Load Balancer của mình. Tất cả những gì bạn làm là bắt đầu thí nghiệm từ FIS console và quan sát cách dịch vụ của bạn phản hồi với một trường hợp lỗi cục bộ được kiểm soát.

Trong ảnh chụp màn hình sau, bạn sẽ thấy quy tắc **Listener ALB** ban đầu chỉ được cấu hình với target group mặc định.

![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/13/Before.png)
  
_Hình 4 ALB listener rule trước khi thí nghiệm bắt đầu, hiển thị một target group duy nhất nhận 100% lưu lượng truy cập._

Sau khi thí nghiệm bắt đầu, AWS FIS sửa đổi rule để chia lưu lượng truy cập — như được hiển thị trong ảnh chụp màn hình Sau.
  
![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/13/After.png)

_Hình 5 ALB listener rule sau khi thí nghiệm bắt đầu, hiển thị một target group mới với Lambda được cấu hình để nhận 50% lưu lượng truy cập và phản hồi bằng một mã lỗi được xác định trước._

## **Bước 4: Quan sát và phân tích kết quả**

Bạn có thể xác thực thí nghiệm bằng cách làm mới **ALB DNS** trong trình duyệt của bạn hoặc chạy một **curl loop**:
    
    
  ```bash
while true; do curl -s http://<your-alb-dns-name>; sleep 1; done
```
![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/13/curl_test.gif)
  
_Hình 6 Đầu ra CLI động hiển thị các yêu cầu lặp lại đến URL ALB trong một vòng lặp để chứng minh cách giải pháp tiêm lỗi._

Bạn sẽ thấy đầu ra xen kẽ như:

  * Backend service is healthy (dịch vụ backend đang hoạt động tốt)
  * Service Unavailable – Simulated Error Response (Lambda)


Bạn có thể giám sát Amazon CloudWatch Logs để xem các chỉ số thực thi hàm Lambda và hành vi của ứng dụng (logic thử lại, cơ chế chuyển đổi dự phòng)

**Lưu ý:** Sau khi bắt đầu thí nghiệm, có thể mất đến một phút trước khi target group mới được gắn vào ALB và lưu lượng truy cập bắt đầu định tuyến đến hàm Lambda. Trong khoảng thời gian ngắn này, tất cả các yêu cầu có thể tiếp tục đến dịch vụ backend ban đầu.

## **Cơ chế khôi phục**

Thí nghiệm nhằm mục đích giúp ích cho các hoạt động khôi phục, mặc dù nên kiểm tra trong môi trường của bạn:

  * ALB rule sẽ tự động được khôi phục vào cuối thời gian kiểm tra.
  * Target group tạm thời được gỡ bỏ và xóa để ngăn chặn bất kỳ cấu hình còn sót lại nào.
  * Nếu thí nghiệm bị hủy, quy trình khôi phục sẽ đưa hệ thống trở lại trạng thái ban đầu.



## **Các điểm cần cân nhắc**

Bài đăng này cung cấp thông tin kỹ thuật và cấu hình ví dụ. Việc triển khai trong môi trường của bạn có thể yêu cầu thêm các cân nhắc về bảo mật, tuân thủ và kỹ thuật. Luôn kiểm tra kỹ lưỡng trước trong các môi trường phi sản xuất.

## **Dọn dẹp**

Để tránh phát sinh chi phí trong tương lai, hãy xóa các tài nguyên đã triển khai:
    
    
  ```bash
aws cloudformation delete-stack --stack-name alb-fis-experiment
```
## **Kết luận**

Trong bài đăng này, chúng tôi đã chứng minh cách mở rộng khả năng của AWS FIS bằng cách mô phỏng lỗi cục bộ cho các workload đằng sau ALB bằng cách sử dụng Lambda. Giải pháp này cho phép các nhóm kiểm tra khả năng phục hồi của ứng dụng đối với các lỗi không liên tục mà không gây ra sự cố ngừng hoạt động hoàn toàn. Bằng cách tận dụng AWS FIS, Lambda, và các ALB routing rules, bạn có thể tạo ra các kịch bản lỗi có kiểm soát và tăng cường độ vững chắc của hệ thống.

Để tìm hiểu thêm, hãy khám phá các tài nguyên sau:

  * [Tài liệu AWS Fault Injection Service](https://docs.aws.amazon.com/fis/latest/userguide/)
  * [Tài liệu Amazon ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)
  * [Sử dụng SSM Systems Manager document với AWS FIS](https://docs.aws.amazon.com/fis/latest/userguide/actions-ssm-agent.html)



Bắt đầu với [CloudFormation template](https://d2908q01vomqb2.cloudfront.net/artifacts/MTBlog/cloudops-1899/fis_template.yaml) và chia sẻ kinh nghiệm của bạn trong phần bình luận bên dưới.

TAGS: [aws fault injection simulator](https://aws.amazon.com/blogs/mt/tag/aws-fault-injection-simulator/), [chaos engineering](https://aws.amazon.com/blogs/mt/tag/chaos-engineering/)

### Ozgur Canibeyaz

![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/13/canibeya.jpeg)


Ozgur là Senior Technical Account Manager tại Amazon Web Services với 8 năm kinh nghiệm. Ozgur giúp khách hàng tối ưu hóa việc sử dụng AWS của họ bằng cách xử lý các thách thức kỹ thuật, khám phá các cơ hội tiết kiệm chi phí, đạt được sự xuất sắc trong vận hành và xây dựng các dịch vụ sáng tạo bằng các sản phẩm AWS.

### Pablo Colazurdo
![](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/06/15/pablo.png)

Pablo là Principal Solutions Architect tại AWS, nơi anh ấy thích giúp khách hàng ra mắt các dự án thành công trên Cloud. Anh ấy có nhiều năm kinh nghiệm làm việc với nhiều công nghệ đa dạng và đam mê học hỏi những điều mới. Pablo lớn lên ở Argentina nhưng hiện đang tận hưởng cơn mưa ở Ireland trong khi nghe nhạc, đọc sách hoặc chơi D&D với các con.
