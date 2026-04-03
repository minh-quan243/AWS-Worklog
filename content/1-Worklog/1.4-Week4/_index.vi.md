---
title: "Worklog Tuần 4"
date: 2026-02-02
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Nắm vững các thành phần mạng trong **AWS** và cách thiết kế kiến trúc mạng cơ bản.
* Hiểu rõ cơ chế bảo mật ở mức network và cách kiểm soát truy cập tài nguyên.
* Làm quen với các dịch vụ hỗ trợ tăng khả năng chịu tải và tối ưu hiệu năng hệ thống.
* Củng cố kiến thức thông qua các bài lab thực hành thực tế.

---

### Các công việc đã thực hiện trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu về **VPC (Virtual Private Cloud)** <br>&emsp; + Khái niệm mạng riêng trên cloud <br>&emsp; + CIDR Block và cách chia subnet <br>&emsp; + Public Subnet vs Private Subnet <br>&emsp; + Route Tables và cách định tuyến <br>&emsp; + Internet Gateway và NAT Gateway <br> - Thực hành <br>&emsp; + Tạo VPC mới <br>&emsp; + Tạo và cấu hình Subnets <br>&emsp; + Thiết lập Route Table cho public/private <br>&emsp; + Kết nối Internet cho EC2 instance | 02/02/2026 | 02/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Nghiên cứu cơ chế bảo mật mạng <br>&emsp; + **Security Groups** (stateful firewall) <br>&emsp; + **Network ACLs** (stateless firewall) <br>&emsp; + So sánh Security Groups và NACLs <br> - Thực hành <br>&emsp; + Tạo rule cho phép/deny traffic <br>&emsp; + Kiểm tra truy cập SSH/HTTP <br>&emsp; + Áp dụng rule để giới hạn IP truy cập | 03/02/2026 | 03/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Thực hiện các lab tổng hợp <br>&emsp; + Triển khai EC2 trong VPC đã tạo <br>&emsp; + Kết nối EC2 với Internet và kiểm tra truy cập <br>&emsp; + Kết hợp S3 với EC2 để lưu trữ dữ liệu <br> - Xử lý lỗi phát sinh trong quá trình cấu hình mạng và truy cập | 04/02/2026 | 04/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Tìm hiểu dịch vụ **Elastic Load Balancing (ELB)** <br>&emsp; + Khái niệm load balancing <br>&emsp; + Application Load Balancer (ALB) <br>&emsp; + Network Load Balancer (NLB) <br>&emsp; + Listener và Target Group <br> - Thực hành <br>&emsp; + Tạo Load Balancer <br>&emsp; + Kết nối với EC2 instances <br>&emsp; + Test phân phối traffic <br> - Kết hợp với **Auto Scaling Group** để tăng tính linh hoạt | 05/02/2026 | 05/02/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Khám phá dịch vụ **ElastiCache** <br>&emsp; + Khái niệm caching và vai trò trong hệ thống <br>&emsp; + Redis và Memcached <br>&emsp; + Use case thực tế của caching <br> - Thực hành <br>&emsp; + Tạo cache instance (Redis) <br>&emsp; + Kết nối và kiểm tra hoạt động cơ bản <br>&emsp; + Hiểu cách cache giúp giảm tải database | 06/02/2026 | 06/02/2026 | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 4:

* Hiểu rõ cách xây dựng và cấu hình mạng trong **AWS**:
  * Nắm được cấu trúc của một **VPC hoàn chỉnh**
  * Phân biệt và triển khai Public/Private Subnet
  * Thiết lập routing thông qua Route Tables
  * Kết nối Internet bằng Internet Gateway và NAT Gateway

* Nắm chắc các cơ chế bảo mật:
  * Phân biệt rõ **Security Groups** và **Network ACLs**
  * Hiểu cách hoạt động stateful vs stateless
  * Thiết lập rule phù hợp để kiểm soát truy cập tài nguyên

* Củng cố kiến thức thông qua lab:
  * Triển khai hệ thống EC2 trong môi trường VPC thực tế
  * Xử lý lỗi liên quan đến network và permission
  * Hiểu rõ luồng traffic trong hệ thống

* Hiểu nguyên lý cân bằng tải:
  * Sử dụng **Load Balancer** để phân phối traffic
  * Kết hợp với **Auto Scaling** để tăng khả năng chịu tải
  * Nhận thức được vai trò của load balancing trong hệ thống production

* Tiếp cận dịch vụ **ElastiCache**:
  * Hiểu vai trò của caching trong việc tăng hiệu năng
  * Biết cách giảm tải cho database backend
  * Thực hành triển khai cache cơ bản với Redis

* Hiểu rõ hơn về kiến trúc hệ thống:
  * VPC → Network foundation
  * Security Groups/NACLs → Security layer
  * ELB + Auto Scaling → High availability
  * ElastiCache → Performance optimization