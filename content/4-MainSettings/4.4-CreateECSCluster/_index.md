---
title : "Create ECS Cluster"
date :  "`r Sys.Date()`" 
weight : 13 
chapter : false
pre : " <b> 4.4. </b> "
---

### Create ECS Cluster

#### Step 1. Go to ECS Dashboard

Navigate to ECS dashboard at this link: [ECS Dashboard](https://console.aws.amazon.com/ecs/v2/getStarted). Then click to **Cluster**.

![VPC](../../images/4.3-defineecstask/2/ecs_1.png)

#### Step 2. Start to create Cluster

Click to **Create Cluster** button.

![VPC](../../images/4.3-defineecstask/2/ecs_2.png)

#### Step 3. Set a name for your cluster

Enter a unique name for your cluster.

![VPC](../../images/4.3-defineecstask/2/ecs_3.png)

#### Step 4. Choose infrastructure

I choose Fargate in this case because I don't want to manage servers. And remember, you choosed Fargate for your Task definition before.

![VPC](../../images/4.3-defineecstask/2/ecs_4.png)

#### Step 5. Complete creation

Click **Create** to create ECS cluster.

![VPC](../../images/4.3-defineecstask/2/ecs_5.png)

### Create ECS Service


#### Step 1: Go to Clusters

Click to cluster that you want to create service. Then you will go to cluster detail page.

![VPC](../../images/4.3-defineecstask/3/ecs_1.png)

#### Step 2: Start to create service

In cluster detail page, scroll down to **Services** section. Then Click to **Create** button.

![VPC](../../images/4.3-defineecstask/3/ecs_2.png)

#### Step 3: Config service environment

I want to create a simple guide for you to start with ECS. So I choose **Launch type**.

The keys differences:

- **Launch type**: If development simplicity and cost-effectiveness are your priorities, using a launch type (Fargate or EC2, depending on your preference) might be sufficient.

- **Capacity provider strategy**: If you need precise resource allocation, advanced scaling, and potentially cost optimization, employing a capacity provider strategy is a better option.

![VPC](../../images/4.3-defineecstask/3/ecs_3.png)

#### Step 4: Config for deployment

**Application type**: 

- **Service** type is more suitable if you want to host a website.

- **Task** type is more suitable if you want to run batch job.

> As I discuss ealier, we will hosting a website by ECS, so **Service** is a great choice here.

**Task definition**:

Choose the task definition that you have created in previous steps.

**Service Name**

Give a unique name for your service.

![VPC](../../images/4.3-defineecstask/3/ecs_4.png)

#### Step 5: Choose a number of tasks

Enter a number that specify the tasks you want to run.

![VPC](../../images/4.3-defineecstask/3/ecs_5.png)

#### Step 6: Configure VPC

Choose the VPC you created in prerequisites section, then choose the **public subnets** to run service. 

![VPC](../../images/4.3-defineecstask/3/ecs_6.png)

#### Step 6: Configure Security Group (SG)

Create a new SG to protect your resource, but still open necessary port to allow user access your website. I open port **80** for my website.

![VPC](../../images/4.3-defineecstask/3/ecs_7.png)

#### Step 7: Complete creation

Click **Create** button to create ECS service.

![VPC](../../images/4.3-defineecstask/3/ecs_8.png)
