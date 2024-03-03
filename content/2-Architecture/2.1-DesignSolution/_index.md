---
title : "Designing CI/CD Solution"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 2.1. </b> "
---

## Designing CI/CD Solution

![Architect](../../images/cicd.png)

The architecture outlines the CI/CD pipeline for your project hosted on GitLab, CI/CD pipeline is defined by **.gitlab-ci.yml** file.

#### Pipeline Phases:

- **Build**: This phase focuses on building your project from the source code within the GitLab repository.

- **Push**: This phase involves creating a container image based on your build output and pushing it to your **Elastic Container Registry (ECR)** repository.

- **Deploy**: This phase updates your **Elastic Container Service (ECS)** with the newly created image from ECR.

#### Job Execution Environment:

The CI/CD jobs will be executed on an **Elastic Compute Cloud (EC2)** instance registered as a **GitLab Runner**. This setup allows GitLab to delegate task execution to the EC2 instance.

#### How it work?

- 1. User commit code to GitLab repository.
- 2. GitLab detect changes and run CI/CD pipeline (The pipeline is defined in .gitlab-ci.yml)
- 3. GitLab Runner build project from source code.
- 4. After build completed, GitLab runner build container image and push image to ECR.
- 5. After image is pushed to ECR, GitLab runner run the deploy phase, that will be update the ECS service with latest version of container image.
- 6. Service complete and newer verion of website is release.