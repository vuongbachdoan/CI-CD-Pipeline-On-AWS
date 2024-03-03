---
title : "Thiết kế Giải pháp CI/CD"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 2.1. </b> "
---

## Thiết kế một Giải pháp CI/CD

![Kiến trúc](../../images/cicd.png)

Kiến trúc mô tả quy trình CI/CD cho dự án của bạn được lưu trữ trên GitLab, CI/CD pipeline được định nghĩa thông qua file **.gitlab-ci.yml**.

#### Các giai đoạn trong CI/CD pipeline:

- **Xây dựng (Build)**: Giai đoạn này tập trung vào việc xây dựng dự án từ mã nguồn trên GitLab.
- **Đẩy (Push)**: Giai đoạn này liên quan đến việc tạo container image dựa trên đầu ra của quá trình build và đẩy nó vào **Elastic Container Registry (ECR)**.
- **Triển khai (Deploy)**: Giai đoạn này cập nhật **Elastic Container Service (ECS)** service của bạn với container image mới được tạo từ ECR.

#### Môi trường thực thi công việc:

Các công việc CI/CD sẽ được thực hiện trên một máy chủ **Elastic Compute Cloud (EC2)** đã đăng ký làm GitLab Runner. Thiết lập này cho phép GitLab giao việc thực thi cho máy chủ EC2.

#### Cách hoạt động?

- 1. Người dùng commit mã vào repository GitLab.
- 2. GitLab phát hiện các thay đổi và chạy quy trình CI/CD (quy trình được định nghĩa trong tệp **.gitlab-ci.yml**).
- 3. GitLab Runner xây dựng dự án từ mã nguồn.
- 4. Sau khi xây dựng hoàn tất, GitLab Runner tạo container image và đẩy image lên ECR.
- 5. Sau khi image được đẩy lên ECR, GitLab Runner chạy giai đoạn triển khai, cập nhật dịch vụ ECS với phiên bản mới nhất của container image.
- 6. Dịch vụ hoàn thành và phiên bản mới của trang web được phát hành.