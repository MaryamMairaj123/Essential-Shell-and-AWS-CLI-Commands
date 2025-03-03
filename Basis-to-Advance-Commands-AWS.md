---
title: "Streamline Cloud Management: Top Shell and AWS CLI Commands - Basic to Advance"
seoTitle: "Cloud Management: Key Shell & AWS CLI Commands"
seoDescription: "Learn essential shell and AWS CLI commands to efficiently manage cloud resources, automate tasks, and enhance your cloud engineering skills"
datePublished: Thu Jan 16 2025 09:17:05 GMT+0000 (Coordinated Universal Time)
cuid: cm5z4a23a000g09i6e76ecdd2
slug: streamline-cloud-management-top-shell-and-aws-cli-commands-basic-to-advance
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/zGuBURGGmdY/upload/7fa4eca242e4db871caeb3ec0adcf719.jpeg
tags: linux, aws, aws-lambda, aws-ecs, iam, aws-cloudformation, shell-script, s3-bucket, ec2-instance, aws-auto-scaling, aws-lifecycle

---

## Introduction

As a cloud engineer, mastering the command line is a fundamental skill that allows you to manage cloud resources efficiently. From navigating file systems to automating infrastructure with AWS CLI (Command Line Interface), shell commands enable you to streamline workflows and interact with cloud services quickly. In this post, I will explore essential shell commands and AWS CLI commands that every cloud engineer should know.

## **Basic Shell Commands**

Let's start with some fundamental shell commands that every cloud engineer should be comfortable with. These commands help in managing directories, files, and processes on a server.

### 1\. **Navigating Directories and Files**

* `ls` – Lists the contents of a directory.
    
* ```bash
        ls
        ls -l
        ls -a
    ```
    
    You can add options like `ls -l` for a detailed listing or `ls -a` to see hidden files.
    
* `cd [directory]` – Changes the current directory.
    
    ```bash
    cd /var/logs
    ```
    
* `mkdir [directory]` – Creates a new directory.
    
    ```bash
    mkdir backups
    ```
    
* `rm [file/folder]` – Removes a file or directory (use `-r` for directories).
    
    ```bash
    rm -r old_files
    ```
    

### 2\. **Managing Files**

* `cp [source] [destination]` – Copies files or directories.
    
    ```bash
    cp index.html /var/www/html/
    ```
    
* `mv [source] [destination]` – Moves or renames files.
    
    ```bash
    mv old_name.txt new_name.txt
    ```
    
* `touch [file]` – Creates an empty file.
    
    ```bash
    touch readme.md
    ```
    

### 3\. **File Permissions**

* `chmod [permissions] [file]` – Changes file permissions.
    
    ```bash
    chmod 755 script.sh
    ```
    
* `chown [owner]:[group] [file]` – Changes file ownership.
    
    ```bash
    chown root:root my_file.txt
    ```
    

### 4\. **System Monitoring**

* `top` – Shows real-time system statistics (CPU, memory, processes).
    
    ```bash
    top
    ```
    
* `df -h` – Displays disk usage.
    
    ```bash
    df -h
    ```
    
* `free -m` – Shows memory usage.free -m
    
* ```bash
        free -m
    ```
    

---

## **Introduction to AWS CLI**

The AWS CLI allows you to interact with AWS services directly from the command line. This is essential for automating tasks, managing cloud resources, and integrating your infrastructure into scripts. The AWS CLI provides a unified interface for running commands that control AWS services like EC2, S3, and IAM.

### **Setting Up an EC2 Instance Using AWS CLI**

#### Step 1: Configure AWS CLI

Before using AWS CLI, you need to configure it with your AWS credentials.

**Command:**

```bash
aws configure
```

You will be prompted to enter:

* **AWS Access Key ID**
    
* **AWS Secret Access Key**
    
* **Default region name** (e.g., `us-west-2`)
    
* **Default output format** (optional, e.g., `json`)
    

---

#### Step 2: Create a Key Pair

A key pair is required to securely connect to your EC2 instance via SSH.

**Command**:

```bash
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
```

* **Explanation**: This command generates a new key pair and saves it to a file called `MyKeyPair.pem`
    

---

#### Step 3: Launch an EC2 Instance

Use the following command to launch an EC2 instance.

**Command**:

```bash
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --instance-type t2.micro \
    --key-name MyKeyPair \
    --security-group-ids sg-0123456789abcdef0 \
    --subnet-id subnet-0123456789abcdef0 \
    --associate-public-ip-address
```

* **Explanation**:
    
    * `image-id`: The ID of the AMI (Amazon Machine Image) for the operating system you want.
        
    * `instance-type`: EC2 instance type (e.g., `t2.micro` for free-tier usage).
        
    * `key-name`: Your previously created key pair.
        
    * `security-group-ids`: The security group that defines the access rules.
        
    * `subnet-id`: The subnet to launch the instance in.
        

---

#### Step 4: Connect to Your EC2 Instance

After launching your instance, connect to it using the SSH command.

**Command**:

```bash
ssh -i "MyKeyPair.pem" ec2-user@ec2-xx-xxx-xxx-xx.compute-1.amazonaws.com
```

* **Explanation**: Replace the IP address with the public IP of your instance.
    

---

### 1\. **Managing EC2 Instances**

The following commands will help you manage EC2 instances (virtual machines) using the AWS CLI.

* **List EC2 Instances:**
    
    ```bash
    aws ec2 describe-instances
    ```
    
    This command provides details of all your running EC2 instances, including instance IDs, state, and public IPs.
    
* **Start an EC2 Instance:**
    
    ```bash
    aws ec2 start-instances --instance-ids i-0abcd1234efgh5678
    ```
    
* **Stop an EC2 Instance:**
    
    ```bash
    aws ec2 stop-instances --instance-ids i-0abcd1234efgh5678
    ```
    
* **Terminate an EC2 Instance:**
    
    ```bash
    aws ec2 terminate-instances --instance-ids i-0abcd1234efgh5678
    ```
    

---

### 2\. **Working with S3 Buckets**

S3 (Simple Storage Service) is AWS's object storage service, and the AWS CLI provides simple commands to manage files in S3.

* **Create an S3 Bucket:**
    
    ```bash
    aws s3 mb s3://my-new-bucket
    ```
    
* **Upload a File to S3:**
    
    ```bash
    aws s3 cp myfile.txt s3://my-new-bucket/
    ```
    
* **List Contents of an S3 Bucket:**
    
    ```bash
    aws s3 ls s3://my-new-bucket/
    ```
    
* **Download a File from S3:**
    
    ```bash
    aws s3 cp s3://my-new-bucket/myfile.txt ./
    ```
    
* **Sync a Local Directory with S3:**
    
    ```bash
    aws s3 sync ./local-folder s3://my-new-bucket/
    ```
    

---

### 3\. **IAM (Identity and Access Management)**

IAM is a crucial service for managing users and permissions in AWS.

* **List All IAM Users:**
    
    ```bash
    aws iam list-users
    ```
    
* **Create a New IAM User:**
    
    ```bash
    aws iam create-user --user-name new_user
    ```
    
* **Attach a Policy to a User:**
    
    ```bash
    aws iam attach-user-policy --user-name new_user --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
    ```
    

---

### 4\. **Lambda Function Management**

AWS Lambda allows you to run code without provisioning servers.

* **Invoke a Lambda Function:**
    
    ```bash
    aws lambda invoke --function-name my-function out.txt
    ```
    
* **List All Lambda Functions:**
    
    ```bash
    aws lambda list-functions
    ```
    

---

## **Advanced AWS CLI Commands**

Let’s dive into some advanced AWS CLI commands for more experienced cloud engineers. These commands are crucial for complex infrastructure management and automation.

### 1\. **Create an Auto-Scaling Group**

An Auto-Scaling Group ensures that you have the right number of EC2 instances running to handle the load for your application.

**Command:**

```bash
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name my-asg \
    --launch-configuration-name my-launch-config \
    --min-size 1 \
    --max-size 5 \
    --desired-capacity 2 \
    --vpc-zone-identifier subnet-0123456789abcdef0
```

* **Explanation**: This command creates an Auto-Scaling Group with a minimum of 1 instance, a maximum of 5, and a desired capacity of 2.
    

---

### 2\. **Create a CloudFormation Stack**

AWS CloudFormation automates the provisioning of infrastructure. You can create, update, or delete an entire stack with a single command.

**Command:**

```bash
aws cloudformation create-stack \
    --stack-name my-stack \
    --template-body file://template.json \
    --parameters ParameterKey=InstanceType,ParameterValue=t2.micro
```

* **Explanation**: This command creates a CloudFormation stack from a template file (`template.json`), with parameters such as the instance type.
    

---

### 3\. **Advanced S3 Commands: Lifecycle Policies**

You can use AWS CLI to automate lifecycle policies for S3, helping to move objects to different storage classes or delete them after a certain period.

**Command:**

```bash
aws s3api put-bucket-lifecycle-configuration \
    --bucket my-bucket \
    --lifecycle-configuration file://lifecycle.json
```

* **Explanation**: This command applies a lifecycle policy defined in `lifecycle.json` to move objects to Glacier after 30 days and delete them after 365 days.
    

---

### 4\. **ECS (Elastic Container Service) Cluster Management**

For managing containerized applications, ECS helps you orchestrate Docker containers. Here’s how to list tasks running in an ECS cluster.

**Command:**

```bash
aws ecs list-tasks --cluster my-cluster
```

* **Explanation**: This command lists the tasks running in an ECS cluster named `my-cluster`.
    

---

### 5\. **Lambda Function Deployment**

You can update your AWS Lambda function’s code directly from the CLI.

**Command:**

```bash
aws lambda update-function-code \
    --function-name my-function \
    --zip-file fileb://function.zip
```

* **Explanation**: This command updates the code for the Lambda function `my-function` by uploading a new zip file [`function.zip`](http://function.zip).
    

---

## **Automation and Scripting with AWS CLI**

One of the AWS CLI's most powerful aspects is its ability to be integrated into scripts. For example, you can automate backups, resource scaling, or deployment processes using shell scripts combined with AWS CLI commands.

### Example Script: Automated EC2 Start and Stop

Here's a simple script that starts an EC2 instance, waits for 10 minutes, and then stops it.

```bash
#!/bin/bash

# Start EC2 instance
aws ec2 start-instances --instance-ids i-0abcd1234efgh5678
echo "Instance started."

# Wait for 10 minutes
sleep 600

# Stop EC2 instance
aws ec2 stop-instances --instance-ids i-0abcd1234efgh5678
echo "Instance stopped."
```

This script can be scheduled `cron` to automate regular instance start/stop processes, saving costs when you don't need the instance running continuously.

---

## **Conclusion**

Mastering both basic and advanced shell commands and the AWS CLI is essential for any cloud engineer aiming to work efficiently in cloud environments. These commands empower you to interact directly with AWS resources, automate tasks, and manage infrastructure flexibly and powerfully. Whether you are managing EC2 instances, uploading files to S3, or handling IAM users, the command line provides the control and speed you need to succeed in cloud computing.

If you found this post helpful, feel free to leave comments or ask questions about other AWS CLI commands. Let's continue exploring the cloud together!!
