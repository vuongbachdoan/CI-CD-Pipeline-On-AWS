---
title : "Thiết lập ban đầu"
date :  "`r Sys.Date()`" 
weight : 7 
chapter : false
pre : " <b> 3. </b> "
---

## Thiết lập ban đầu

Bạn cần một **Virtual Private Cloud (VPC)** để chạy các dịch vụ AWS bên trong nó. Việc đặt các dịch vụ của bạn bên trong VPC sẽ đảm bảo tính bảo mật, đồng thời bạn cũng có thể thực hiện các phương án như cài đặt **Security Group (SG)** hay **Network Access Control List (NACL)** để bảo vệ các tài nguyên của mình.

Đồng thời, một số dịch vụ như **Elastic Compute Cloud (EC2)**, **Elastic Container Service (ECS)** sẽ yêu cầu bạn đặt những dịch vụ này vào trong 1 VPC. 

> **Note:** Tất nhiên bạn có thể xài VPC mặt định, nhưng tôi khuyến khích bạn tạo 1 VPC mới, bởi vì mỗi VPC nên chỉ được sử dụng cho một mục đích riêng (việc bạn đặt tất cả dịch vụ của mình vào trong VPC mặc định sẽ khiến cho việc quản lý VPC của bạn trở nên khó khăn về sau này).

![VPC](../images/aws/VPC.png)