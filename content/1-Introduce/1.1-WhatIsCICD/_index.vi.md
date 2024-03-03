---
title : "CI/CD là gì?"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.1. </b> "
---

### CICD là gì?
 
CI/CD Pipeline là một chuỗi các giai đoạn tự động chuyển đổi mã nguồn của bạn thành một website đang chạy.

#### a. Tích hợp liên tục (CI - Continuous Integration)

- Thay đổi mã nguồn được tự động kéo từ repository Git của bạn.
- Các bài kiểm tra đơn vị (unit test), kiểm tra chất lượng mã (code quality check) và kiểm tra tích hợp (integration test) được thực hiện.
- Nếu thành công, mã nguồn được xây dựng thành một sản phẩm triển khai (deployable artifact).

#### b. Phát hành liên tục (CD - Continuous Delivery)

- Sản phẩm triển khai được triển khai đến môi trường staging để kiểm tra thủ công và phê duyệt.
- Sau khi được phê duyệt, sản phẩm triển khai được triển khai đến môi trường sản xuất (production).
- Kiểm tra hiệu suất và hoạt động được thực hiện liên tục.



