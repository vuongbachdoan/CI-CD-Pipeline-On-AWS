---
title : "Clean Up Resources"
date :  "`r Sys.Date()`" 
weight : 17 
chapter : false
pre : " <b> 5. </b> "
---

### Here is some service need to cleanup

#### Delete EC2 instance (Instance connect to your GitLab Runner)

Back to EC2 Dashboard, go to **Instances** from left sidebar and delete.

![Clean Up](../images/7-cleanup/clean_1.png)

#### Delete ECR repository

Back to ECR Dashboard, go to **Repositories** from left sidebar and delete.

![Clean Up](../images/7-cleanup/clean_2.png)

#### Delete ECS Cluster 

Back to ECS Dashboard, delete your task and service first, then delete your cluster.

![Clean Up](../images/7-cleanup/clean_3.png)

#### Delete ECS Task definition

Back to ECS Dashboard, go to **Task definition** from left sidebar and delete.

![Clean Up](../images/7-cleanup/clean_4.png)

#### Delete NAT Gateway

Back to your VPC, go to **NAT Gateway** from left sidebar and delete.

![Clean Up](../images/7-cleanup/clean_5.png)

#### Delete S3 Endpoint

Back to your VPC, go to **Endpoints** from left sidebar and delete.

![Clean Up](../images/7-cleanup/clean_6.png)

#### Delete Elastic IP

Back to your VPC, go to **Elastic IPs** from left sidebar and delete.

![Clean Up](../images/7-cleanup/clean_7.png)

#### Delete Elstic Network Interface (ENI)

Back to EC2 Dashboard, go to **Network Interface** and delete.

![Clean Up](../images/7-cleanup/clean_8.png)

#### Delete VPC

Back to your VPC, select the VPC you want to delete and delete.

![Clean Up](../images/7-cleanup/clean_9.png)

🎉 Everything is done. Hope you have a good experience with this workshop!