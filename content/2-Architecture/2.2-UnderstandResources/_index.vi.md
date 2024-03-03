---
title : "Tìm hiểu về các tài nguyên trong kiến trúc này"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b> 2.2. </b> "
---

#### Elastic Compute Cloud (EC2)

![Diagram](../../images/aws/EC2.png)

Dịch vụ AWS này cung cấp các máy chủ ảo có khả năng mở rộng (các EC2 instances) trên cloud. GitLab Runner của bạn sẽ được cài đặt trên một EC2 instance, cho phép GitLab giao việc thực thi CI/CD.

#### Elastic Container Registry

![Diagram](../../images/aws/ECR.png)

Dịch vụ AWS này hoạt động như một kho lưu trữ an toàn cho container image của bạn. Bạn sẽ đẩy container image được xây dựng trong quy trình CI/CD vào kho ECR của bạn.

#### Elastic Container Service

![Diagram](../../images/aws/ECS.png)

Dịch vụ AWS này cung cấp một nền tảng để chạy và quản lý các ứng dụng được đóng gói trong container. Bạn sẽ sử dụng ECS để triển khai ứng dụng web của bạn sau khi cập nhật với container image mới được tạo từ ECR.

#### GitLab

![Diagram](../../images/gitlab.png)

Nền tảng này là hệ thống quản lý phiên bản mã nguồn cho dự án của bạn. Nó lưu trữ một cách an toàn mã nguồn của bạn và theo dõi các thay đổi theo thời gian.

#### .gitlab-ci.yml

Tệp YAML này xác định các giai đoạn và nhiệm vụ cụ thể trong quy trình CI/CD của bạn, chỉ định các hành động cần thực hiện ở mỗi giai đoạn.