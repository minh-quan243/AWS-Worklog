---
title: "Route 53 DNS"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.3.2. </b> "
---

Amazon Route 53 cung cấp DNS cho tên miền tùy chỉnh của nền tảng Voice Summarizer. Trong phần này, bạn sẽ tạo một **Public Hosted Zone** (Vùng lưu trữ công khai) cho tên miền của mình, ủy quyền nó cho các máy chủ định danh (nameservers) của Route 53 thông qua nhà đăng ký tên miền của bạn, sau đó tạo một **bản ghi bí danh A (alias A record)** để định tuyến tên miền của bạn đến Application Load Balancer.

Khi hoàn tất, người dùng sẽ truy cập nền tảng thông qua tên miền tùy chỉnh của bạn (ví dụ: `voice.yourdomain.com`) thay vì tên DNS gốc của ALB.

{{% notice info %}}
**Điều kiện tiên quyết cho phần này:**
- Bạn phải sở hữu một tên miền đã được đăng ký với bất kỳ nhà đăng ký tên miền nào (ví dụ: Namecheap, GoDaddy, Google Domains, hoặc AWS Route 53 Registrar).
- **Application Load Balancer** phải được tạo trước đó (Phần 7), vì bạn sẽ cần tên DNS của nó cho bản ghi bí danh.
{{% /notice %}}

---

#### Bước 1 — Đi tới Hosted Zones và Tạo

Điều hướng đến **Route 53** trong AWS Console. Ở thanh bên trái, nhấp vào **Hosted zones**, sau đó nhấp vào **Create hosted zone**.

![Đi tới Hosted Zones và Tạo](/images/4-Workshop/Route53/B1_Goto_Route53.png)

---

#### Bước 2 — Nhập Tên miền của Bạn và Chọn Public Hosted Zone

Điền các cài đặt Hosted Zone:

- **Domain name**: Nhập tên miền tùy chỉnh của bạn, ví dụ: `yourdomain.com` hoặc `voice.yourdomain.com`
- **Type**: **Public hosted zone** *(phân giải các truy vấn DNS từ internet công cộng)*
- Giữ nguyên tất cả các cài đặt khác ở mức mặc định

Nhấp vào **Create hosted zone**.

![Nhập Tên miền — Public Hosted Zone](/images/4-Workshop/Route53/B2_Enter_Domain.png)

Sau khi tạo, Route 53 sẽ tự động tạo hai bản ghi bên trong Hosted Zone:
- Một bản ghi **NS** chứa bốn máy chủ định danh (nameservers) của Route 53
- Một bản ghi **SOA**

---

#### Bước 3 — Sao chép các giá trị NS (Nameserver)

Nhấp vào Hosted Zone mới tạo để mở nó. Tìm bản ghi **NS** và sao chép cả bốn giá trị nameserver. Chúng sẽ trông giống như sau:

```
ns-123.awsdns-45.com.
ns-678.awsdns-90.net.
ns-111.awsdns-22.org.
ns-444.awsdns-88.co.uk.
```

Bạn sẽ dán các giá trị này vào phần cài đặt máy chủ định danh của nhà đăng ký tên miền ở bước tiếp theo.

![Sao chép các giá trị NS](/images/4-Workshop/Route53/B3_Copy_Value_Route.png)

---

#### Bước 4 — Thêm các bản ghi NS tại Nhà đăng ký Tên miền của bạn

Đăng nhập vào bảng điều khiển của nhà đăng ký tên miền (ví dụ: Namecheap, GoDaddy, hoặc nơi bạn đã đăng ký tên miền). Điều hướng đến phần cài đặt DNS hoặc Nameserver cho tên miền của bạn.

Thay thế các nameserver hiện tại bằng bốn giá trị NS của Route 53 mà bạn đã sao chép ở Bước 3. Lưu các thay đổi.

{{% notice warning %}}
⚠️ Quá trình cập nhật (propagation) DNS sau khi thay đổi nameserver có thể mất từ vài phút đến 48 giờ, tùy thuộc vào cài đặt TTL của nhà đăng ký. Bạn có thể theo dõi quá trình này bằng các công cụ như [dnschecker.org](https://dnschecker.org).
{{% /notice %}}

![Thêm các bản ghi NS tại Nhà đăng ký](/images/4-Workshop/Route53/B4_Add_NS_Record_To_Ur_Domain.png)

---

#### Bước 5 — Tạo Bản ghi bí danh A (Alias A Record) Trỏ đến ALB

Trở lại Hosted Zone trong Route 53, nhấp vào **Create record** để thêm một bản ghi bí danh ánh xạ tên miền của bạn với Application Load Balancer.

Cấu hình bản ghi như sau:

- **Record name**: Để trống cho tên miền gốc (apex domain) (`yourdomain.com`), hoặc nhập một tên miền phụ (subdomain) chẳng hạn như `api` để thành `api.yourdomain.com`
- **Record type**: **A — Routes traffic to an IPv4 address and some AWS resources**
- **Alias**: ✅ **Bật (Toggle on)**
- **Route traffic to**: **Alias to Application and Classic Load Balancer**
- **Region**: `ap-southeast-1`
- **Load balancer**: Chọn `voice-summarizer-alb` từ menu thả xuống

Nhấp vào **Create records**.

![Tạo Bản ghi bí danh A trỏ đến ALB](/images/4-Workshop/Route53/B5_Create_Alias_Pointing_To_ALB.png)

{{% notice info %}}
**Hai bản ghi cho hai endpoint:** Nếu bạn muốn phục vụ cả frontend và API từ cùng một tên miền bằng cách sử dụng các tên miền phụ, hãy tạo hai bản ghi bí danh:<br>• Tên miền gốc (Apex) hoặc subdomain `www` → Tên miền của ứng dụng Amplify (tìm thông tin này trong Amplify Console → App settings → Domain management)<br>• Subdomain `api` → Tên DNS của ALB<br>Đối với workshop này, chỉ cần một bản ghi bí danh trỏ đến ALB là đủ nếu bạn truy cập frontend trực tiếp qua URL mặc định của Amplify.
{{% /notice %}}

---

{{% notice tip %}}
✅ Route 53 đã được cấu hình. Tên miền của bạn hiện được phân giải thông qua Route 53, và lưu lượng truy cập vào tên miền tùy chỉnh của bạn được định tuyến đến Application Load Balancer.<br>
**Để xác minh:** Chạy lệnh `nslookup yourdomain.com` hoặc `dig yourdomain.com` từ terminal của bạn. Kết quả trả về phải là địa chỉ IP của ALB sau khi quá trình cập nhật DNS hoàn tất.
{{% /notice %}}