---
title : "Prerequisites"
date :  "`r Sys.Date()`" 
weight : 7 
chapter : false
pre : " <b> 3. </b> "
---

![VPC](../images/aws/VPC.png)

You need a **Virtual Private Cloud (VPC)** to run AWS services inside it. Placing your services inside a VPC will ensure security, and you can also implement measures such as **Security Group (SG)** or **Network Access Control List (NACL)** to protect your resources.

At the same time, some services like **Elastic Compute Cloud (EC2)**, **Elastic Container Service (ECS)** will require you to put these services into a VPC.

> **Note:** You can use the default VPC, but I encourage you to create a new VPC, because each VPC should only be used for a specific purpose (putting all your services into the default VPC will make it difficult to manage your VPC later on).



