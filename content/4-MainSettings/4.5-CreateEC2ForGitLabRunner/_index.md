---
title : "Create EC2 for GitLab Runner"
date :  "`r Sys.Date()`" 
weight : 14 
chapter : false
pre : " <b> 4.5. </b> "
---

### Create EC2 instance

#### Step 1. Go to EC2 Dashboard

Navigate to EC2 dashboard at this link: [EC2 Dashboard](https://console.aws.amazon.com/ec2/home). Then click to **Instances**.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_1.png)

#### Step 2. Start to create EC2 instance

Click **Launch instance** to create an instance.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_2.png)

#### Step 3. Enter a name to instance

Enter a unique name for your EC2 instance.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_3.png)

#### Step 4: Choose Amazon machine image (AMI)

Depend on your usage, and OS, you can choose whatever instance you like. For lower cost, I use Linux, and it offers **Amazon Linux 2** for free tier.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_4.png)

#### Step 5: Choose instance type

I choose **t2.micro** for my instance type, if you use more CPU and RAM, you need a larger instance type here.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_5.png)

#### Step 6: Create key pair

If you want to access your instance via local SSH like your local CMD or Powershell, you can create a key here and use later. In my case, I will connect to my instance directly via AWS Console so I choose **Proceed without a key pair**.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_6.png)

#### Step 7: Choose VPC

Choose the VPC you created earlier, then using **public subnets** to help you access instance via its public IP.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_7.png)

#### Step 8: Config Security Group

To protect your instance, you can restrict access to your instance by SG. Here I allow SSH from anywhere, but you can restrict access to allow SSH from your IP address only.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_8.png)

#### Step 9: Add User Data

**User Data** is the script that will run at first time when you instance is starting. I use will it to connect to my GitLab Runner.

Expand the **Advance settings**, then scroll down to bottom you will see **User Data** input box.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_9.png)

Please copy this script and paste to you User Data.

```
sudo su - root
curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
chmod +x /usr/local/bin/gitlab-runner
useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
gitlab-runner install --user=<GITLAB_USER_NAME> --working-directory=/home/gitlab-runner
gitlab-runner start
yum install -y docker
yum install -y git
systemctl enable docker
systemctl start docker
gitlab-runner register -n --url <GITLAB_URL> --registration-token <GITLAB_REGISTRATION_TOKEN> --executor docker --description "Deployment Runner" --docker-image "docker:stable" --tag-list deployment --docker-privileged
gitlab-runner run
```


This script sets up a GitLab Runner on a Linux system. It downloads the runner software, creates a dedicated user for it, and configures it to use Docker to run jobs from your GitLab server.

#### Step 10: Complete creation

Click **Launch instance** to start your EC2 instance.

![VPC](../../images/4.5-createec2forgitlabrunner/ec2_10.png)
