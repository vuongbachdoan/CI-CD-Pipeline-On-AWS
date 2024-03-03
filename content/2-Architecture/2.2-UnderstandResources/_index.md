---
title : "Understanding Resources Using in this Architecture"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b> 2.2. </b> "
---

#### Elastic Compute Cloud (EC2)

![Diagram](../../images/aws/EC2.png)

This AWS service provides scalable virtual servers (EC2 instances) in the cloud. Your GitLab Runner software will be installed on an EC2 instance, allowing GitLab to delegate CI/CD job execution.

#### Elastic Container Registry

![Diagram](../../images/aws/ECR.png)

This managed AWS service acts as a secure repository for your container images. You will push the container image built during the CI/CD process to your ECR repository.

#### Elastic Container Service

![Diagram](../../images/aws/ECS.png)

This AWS service provides a platform for running and managing containerized applications. You will use ECS to deploy your web application after updating it with the newly created container image from ECR.

#### GitLab

![Diagram](../../images/gitlab.png)

This platform serves as the source version control system for your project. It securely stores your code and tracks changes over time.

#### .gitlab-ci.yml

This YAML file defines the specific stages and tasks within your CI/CD pipeline, specifying the actions to be taken on each stage.