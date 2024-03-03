---
title : "Setup GitLab Project"
date :  "`r Sys.Date()`" 
weight : 10
chapter : false
pre : " <b> 4.1. </b> "
---

### Define CI/CD pipeline with .gitlab-ci.yml

Specifies the base image used for all jobs in the pipeline. Here, it's **node:18-alpine**, a lightweight Node.js 18 image based on Alpine Linux.

```
image: node:18-alpine
```

Defines three stages for the pipeline: **build**, **push**, and **deploy**. Jobs will run sequentially within each stage, and stages can be configured to run in parallel.

```
stages:
  - build
  - push
  - deploy
```

Runs a sequence of commands before any job:
- Installs Python 3 and its package manager pip using **apk package** manager.
- Creates a virtual environment for awscli using `python3 -m venv`.
- Activates the virtual environment and installs awscli for interacting with AWS.
- Configures AWS credentials using environment variables `$AWS_ACCESS_KEY_ID` and `$AWS_SECRET_ACCESS_KEY`.

```
before_script:
  - apk add --no-cache python3 py3-pip
  - apk add --no-cache docker
  - python3 -m venv /tmp/awscli-venv
  - source /tmp/awscli-venv/bin/activate
  - pip install awscli
  - aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
  - aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
```

Defines a job named build in the build stage:
- Installs dependencies using `yarn install`.
- Runs `CI=false yarn build` to build the project, likely using a build tool like Webpack or Rollup. Setting `CI=false` will ignore warning in build step.
- Saves the built artifacts in the **build** directory for later use.

```
build:
  stage: build
  script:
    - yarn install
    - CI=false yarn build
  artifacts:
    paths:
      - build/
```

Defines a job named push in the push stage:
- Depends on the successful completion of the build job (dependencies: - **build**).
- Uses a different image, **docker:latest**, which provides necessary tools for building and pushing Docker images.
- Uses a Docker service (**docker:dind**) to run Docker commands within the job itself.
- Authenticates with Amazon ECR (Elastic Container Registry) using AWS CLI commands, retrieving login credentials from the environment variable `$AWS_ACCESS_KEY_ID` and secret.
- Creates a simple Dockerfile that copies the built artifacts (build/) to the image and configures Nginx server to serve them.
- Builds and tags the image with the commit SHA for versioning using `$CI_COMMIT_SHA` environment variable.
- Pushes the image to the specified ECR repository.

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

Defines a job named deploy in the deploy stage:
- Uses AWS CLI to update an ECS (Elastic Container Service) service with a new task definition. This likely deploys the newly built image to the running container instances.
- Only runs this job when the pipeline is triggered by the **prod** branch or tag.

```
deploy:
  stage: deploy
  script:
    - aws ecs update-service --cluster $ECS_CLUSTER_NAME --service $ECS_SERVICE_NAME --task-definition $ECS_TASK_DEFINITION_ARN --desired-count 1 --force-new-deployment
  only:
    - prod
```

### Add project variables

#### Here is some variables need to add:

- `AWS_ACCESS_KEY_ID`: Your AWS access key ID for interacting with AWS services.

- `AWS_SECRET_ACCESS_KEY`: Your AWS secret access key for interacting with AWS services.

- `ECS_CLUSTER_NAME`: The name of your ECS cluster where the service is deployed.

- `ECS_SERVICE_NAME`: The name of the ECS service you want to update.

- `ECS_TASK_DEFINITION_ARN`: The ARN (Amazon Resource Name) of the task definition associated with the service.

Please note this names! Later, when we complete creation of AWS resources, we will back to add a value for these variables.

#### Steps to add variables:

**Step 1:** Go to your project's **Settings** > **CI/CD** > **Variables**.

**Step 2:** Click **Add variable** and fill in the details.

### Create GitLab Runner

#### What is GitLab Runner?

Think of GitLab Runner as a robot that follows your instructions in your GitLab CI/CD pipelines. It can run tests, build your application, and deploy it to production, all automatically

In this workshop, we'll delve into the practical deployment of GitLab Runner on an EC2 instance running Linux

#### Steps to create a GitLab Runner:

**Step 1:** Go to your project in GitLab and open the **Settings** tab.

**Step 2:** Under the **CI/CD** section, expand the **Runners** section.

**Step 3:** Click **New project runner**.

**Step 4:** Select your operating system where you installed the GitLab Runner. Then enter details: 
- **Tags**: you can give a tag here, then later you can set to run CI/CD pipeline on only tags you specified.
- **Description** (optional): Add a note for future reference.
- **Configuration**: Establish rules such as preventing the runner from accepting new jobs, allowing only the protected branch to run the CI/CD pipeline, restricting the use of the current runner to the current project only, and setting a maximum job timeout.

**Step 5:** Click **Create runner**. This will create a runner for your project.

> Please note the **GITLAB_REGISTRATION_TOKEN** (GitLab registration token), you will use this value to connect the GitLab runner from your EC2 instance.
