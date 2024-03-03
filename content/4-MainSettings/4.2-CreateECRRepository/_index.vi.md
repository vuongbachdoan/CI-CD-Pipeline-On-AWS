---
title : "Create ECR Repository"
date :  "`r Sys.Date()`" 
weight : 11 
chapter : false
pre : " <b> 4.2. </b> "
---

### Create ECR Repository

#### Step 1. Go to ECR Dashboard

Navigate to ECR dashboard at this link: [ECR Dashboard](https://console.aws.amazon.com/ecr/get-started). Then click to **Get Started**.

![VPC](../../images/4.2-createecrrepository/ecr_1.png)

#### Step 2. Choose repository type

For easier steps, I will choose **Public** repository in **General settings**. 

![VPC](../../images/4.2-createecrrepository/ecr_2.png)

> If you choosing Private type, you must be authenticate before can pull the image from repository. Just add some line to authenticate with AWS in `.gitlab-ci.yml` if you want to pull image from private ECR repository.

#### Step 3. Enter a unique name for your repository

Enter a name for your repository.

![VPC](../../images/4.2-createecrrepository/ecr_3.png)

#### Step 3. Complete creation

Click **Create repository** to complete creation.

![VPC](../../images/4.2-createecrrepository/ecr_4.png)

After complete creation, you will get repository **name** and **URI**. Please note this value, because your need to add this value to variable `$ECR_HASH` of your **GitLab Variables**.

![VPC](../../images/4.2-createecrrepository/ecr_5.png)

