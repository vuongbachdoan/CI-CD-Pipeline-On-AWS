---
title : "Connect EC2 with GitLab Runner"
date :  "`r Sys.Date()`" 
weight : 15 
chapter : false
pre : " <b> 4.6. </b> "
---

Your instance look good but in **build** phase, but in **push** phase maybe you will get this error:

```
ERROR: Cannot connect to the Docker daemon at tcp://docker:2375. Is the docker daemon running?
ERROR: Job failed: exit code 1
```

Look back our **push** phase:

```
push:
  stage: push
  dependencies:
    - build
  image: docker:latest
  services:
    - docker:dind
  script:
    # Login to ECR using AWS CLI
    - aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/$ECR_HASH

    # Build and push Docker image
    - echo -e "FROM nginx:alpine\nCOPY build/ /usr/share/nginx/html" > Dockerfile
    - echo -e "server {\n\tlisten 80;\n\tlocation / {\n\t\troot /usr/share/nginx/html;\n\t\tindex index.html index.htm;\n\t\ttry_files \$uri \$uri/ /index.html;\n\t}\n\terror_page 404 =200 /index.html;\n}" > default.conf
    - echo -e "COPY default.conf /etc/nginx/conf.d/default.conf" >> Dockerfile
    - docker build -t <IMAGE_NAME>:$CI_COMMIT_SHA .
    - docker tag <IMAGE_NAME>:$CI_COMMIT_SHA public.ecr.aws/$ECR_HASH/<IMAGE_NAME>:$CI_COMMIT_SHA
    - docker push public.ecr.aws/$ECR_HASH/<IMAGE_NAME>:$CI_COMMIT_SHA
```
Problem due to the **push** phase might encounter an error due to a potential incompatibility between the Docker image used for running tasks (docker:latest) and the Docker image being built, tagged, and pushed (nginx:alpine ).

Because the two Docker images might not share the necessary Docker environment for building and pushing images.

The solution is mount the volumes of the two Docker images together to ensure they share a consistent Docker environment.

#### Step 1: Go to Instances

In EC2 dashboard, click **Instances** in left sidebar to go to instance page.

![VPC](../../images/4.6-connectec2withgitlabrunner/ec2_1.png)

#### Step 2: Connect to EC2 instance

Select the instance you want to connect, then click **Connect** button.

![VPC](../../images/4.6-connectec2withgitlabrunner/ec2_2.png)

#### Step 3: Choose intance connect type

I will use EC2 instance connect via AWS console. Then click **Connect** to continue.

![VPC](../../images/4.6-connectec2withgitlabrunner/ec2_3.png)

Now you can see the Linux terminal

![VPC](../../images/4.6-connectec2withgitlabrunner/ec2_4.png)

#### Step 4: Mount docker volume

To mount docker volume, enter following scripts:

```
sudo su - root
sudo nano /etc/gitlab-runner/config.toml
```

Scroll down and find the line of **volume**, then add `/var/run/docker.sock:/var/run/docker.sock` in the array. It will look like this:

![VPC](../../images/4.6-connectec2withgitlabrunner/ec2_5.png)

#### Step 5: Check GitLab Runner status

Back to your GitLab project and check if the GitLab runner status is running.

![VPC](../../images/4.6-connectec2withgitlabrunner/ec2_6.png)

Okay, now you completed all setup. Now let see our result!
