---
title : "Define ECS Task"
date :  "`r Sys.Date()`" 
weight : 12 
chapter : false
pre : " <b> 4.3. </b> "
---

### Create ECS Task Definition

#### Step 1. Go to ECS Dashboard

Navigate to ECS dashboard at this link: [ECS Dashboard](https://console.aws.amazon.com/ecs/v2/getStarted). Then click to **Task definition**.

![VPC](../../images/4.3-defineecstask/1/ecs_1.png)

#### Step 2. Start to create Task Definition

There are 2 options: 

- If you have previous defined task definition, you can use as JSON template.

- I want to create a new Task Definition, so I choose **Create new task definition**.

![VPC](../../images/4.3-defineecstask/1/ecs_2.png)

#### Step 3. Configure infrastructure

Now you need to specify some attribute for your task:

- **Launch Type**: If you want AWS manage underlying infrastructure for you, choose Fargate. Otherwise, choose EC2 if you want to manage infrastructure by your self.

- **OS**: You can choose between Linux, or Windows.

- Choose **CPU** and **Memory** for your task.

- **Role**: If you want your ECS Task can interact with other AWS services, then you can specify a Role here, or create a new one.

![VPC](../../images/4.3-defineecstask/1/ecs_3.png)

#### Step 4. Link ECS task definition with ECR repository

- Name: name of container that will run your task, it can be anything.

- Image URI: is the URI of ECR repository you create previously. ex: repo-uri/<IMAGE_NAME>:<TAG>. 

> Remember the values in your **.gitlab-ci.yml** file? Here is what to use that value, make sure they are same:


```
    - docker build -t <IMAGE_NAME>:$CI_COMMIT_SHA .
    - docker tag <IMAGE_NAME>:$CI_COMMIT_SHA public.ecr.aws/$ECR_HASH/<IMAGE_NAME>:$CI_COMMIT_SHA
    - docker push public.ecr.aws/$ECR_HASH/<IMAGE_NAME>:$CI_COMMIT_SHA
```

Next, **mapping port** of your container with port of image will use. In my case, they will be 80 to 80.

![VPC](../../images/4.3-defineecstask/1/ecs_4.png)

#### Step 5. Complete creation

Click **Create** to create ECS Task definition.

![VPC](../../images/4.3-defineecstask/1/ecs_5.png)
