---
title: "Creating a CI/CD Pipeline with AWS CodePipeline, CodeBuild, and CodeDeploy"
seoTitle: "AWS CI/CD pipeline"
seoDescription: "AWS, CodeDeploy, CodeBuild, CodePipeline, CI/CD, Automation"
datePublished: Tue Jan 28 2025 12:48:46 GMT+0000 (Coordinated Universal Time)
cuid: cm6gh4i3j001n09l22udo3bft
slug: creating-a-cicd-pipeline-with-aws-codepipeline-codebuild-and-codedeploy
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738067696177/a5bd0ad0-47c1-44c7-bba4-ffeb26aa9874.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1738068173259/e1cd0916-e80b-48d6-9d27-d46c8ed6f67b.png
tags: ec2, docker, aws, github, automation, cicd, codedeploy, ci-cd, codepipeline, codebuild

---

In modern DevOps workflows, Continuous Integration and Continuous Deployment (CI/CD) pipelines are essential for automating software build, test, and deployment processes. AWS offers several fully managed services that streamline CI/CD workflows: AWS CodePipeline for orchestrating the process, AWS CodeBuild for building and testing code, and AWS CodeDeploy for automating the deployment across various compute services like EC2, ECS, and Lambda.

This blog post will create a complete CI/CD pipeline using AWS CodePipeline, CodeBuild, and CodeDeploy. This tutorial will help you automate the process of building, testing, and deploying an application to an EC2 instance.

## Step 1: Setting Up the Source Repository

To begin, let’s set up a source repository on GitHub.

### **1\. Create a Repository:**

Go to GitHub and create a new repository for your application. Add the sample application code "[https://github.com/MaryamMairaj123/AWS-DevOps/aws-devops-main/day-14](https://github.com/MaryamMairaj123/AWS-DevOps/aws-devops-main/day-14)" **2\. Create a Personal Access Token:** In GitHub, navigate to Settings &gt; Developer Settings &gt; Personal Access Tokens and generate a token with the necessary permissions. This will allow AWS to connect to your GitHub repository. **3\. Configure GitHub in AWS CodePipeline:** Next, you’ll authorize AWS CodePipeline to connect to your GitHub repository.

## Step 2: Setting Up with the AWS System Manager

Go to the systems manager to secure sensitive information for docker registering. In the systems manager go to the parameters stores and click on Create parameters.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/290fdhn7s6m98peajs8j.png align="left")

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9mri8ezbie71t6w4889g.png align="left")

*I have created three parameters.*

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jtw0wkyd6atm1qhawv9d.png align="left")

## Step 3: Setting Up AWS CodeBuild

AWS CodeBuild compiles your code, runs unit tests, and produces deployable artifacts.

### **1\. Create a CodeBuild Project** Go to the AWS Management Console and navigate to CodeBuild. Click Create Build Project. Enter a name for your project (e.g., MyApp-Build).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/drgmuybqp55wegyh5xav.png align="left")

### **2\. Configure Source** Select GitHub as the source provider and choose the repository you created earlier.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jmrm0gk2snc4jorkgfjl.png align="left")

### **3\. Buildspec File** Define a buildspec.yml version of the build spec that can be used in your CodeBuild project:

```bash
version: 0.2
env:
  parameter-store:
    DOCKER_REGISTRY_USERNAME: "/myapp/docker-credentials/username"
    DOCKER_REGISTRY_PASSWORD: "/myapp/docker-credentials/password"
    DOCKER_REGISTRY_URL: "/myapp/docker-registry/url"

phases:
  install:
    runtime-versions:
      python: 3.11
    commands:
      - pip install -r aws-devops-main/day-14/simple-python-app/requirements.txt

  pre_build:
    commands:
      - echo "Logging in to Docker registry..."
      - echo "$DOCKER_REGISTRY_PASSWORD" | docker login "$DOCKER_REGISTRY_URL" --username "$DOCKER_REGISTRY_USERNAME" --password-stdin

  build:
    commands:
      - cd aws-devops-main/day-14/simple-python-app
      - echo "Building Docker Image"
      - docker build -t "$DOCKER_REGISTRY_URL/$DOCKER_REGISTRY_USERNAME/python-flask-app-service:latest" .
      - echo "Pushing Docker Image to Registry"
      - docker push "$DOCKER_REGISTRY_URL/$DOCKER_REGISTRY_USERNAME/python-flask-app-service:latest"

  post_build:
    commands:
      - echo "Build is Successful"
```

### **4\. IAM Role** Ensure that your CodeBuild project has an IAM role with permissions to access the required services (CodeCommit, S3, CodeDeploy, etc.).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pouo9jvuqxhn6dwlhrgs.png align="left")

## Step 4: Attach IAM policies with SSM

Now go to the IAM Roles and attach the policies of SSM

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/04bi8kzb4dc0n065pjm9.png align="left")

## Step 5: Enable privileged

Go to the project edit and enable privileged and update project.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g58ndwm9wipu3d57ouf9.png align="left")

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c3u1fibkp81hlqotlmuo.png align="left")

## Step 6: Configure Deployment with AWS CodePipeline

### **1\. Go to the AWS code pipeline and create a new code pipeline**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c9xkylilh1t3r81pv1zv.png align="left")

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s48w1vfgqjn3nqvq1zx3.png align="left")

### **2\. Configure Source**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/efaad2z0u8fvn274fopn.png align="left")

### **3\. Add Build Stage**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bk2pb6xnz73idfs1kvqc.png align="left")

### **4\. Skip the deployment stage for now and create the pipeline.**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fwm393enlvwepp4htec3.png align="left")

### **5\. Here is the Docker image**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7cnj8ehvox1xh74t6ttx.png align="left")

The CI part is done here.

***Now Deploying CD part.***

## Step 7: Automating Deployment with AWS CodeDeploy

AWS CodeDeploy automates the deployment of your code to EC2 instances.

### **1\. Set Up EC2 Instances** Launch EC2 instances and install the CodeDeploy agent:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/twkeo40rf50bzbiiw0jz.png align="left")

**Add Tags**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h96ccgqtpeisc6jlkj0f.png align="left")

### **Now go to the terminal to create an agent using CLI follow this link:** [https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html)

```bash
sudo yum install ruby
sudo yum install wget
wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install 
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent start
systemctl status codedeploy-agent
```

### **2\. Create a CodeDeploy Application** In the AWS Management Console, navigate to CodeDeploy and click Create Application. Name the application and select EC2/On-Premises as the compute platform.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cvc2t5k256559wpt0yne.png align="left")

### **3\. create a role** To grant permissions to any service to access another service in AWS, we use IAM roles. Roles are used for services. Create a role and select code deploy then click next and then next

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2d1a922gsa92hn75xoql.png align="left")

### Select the role name and create the role.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9xpbjlgtqf3ymoskvf7b.png align="left")

### **Now go to the modified IAM roles.**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4a250gxlzjao3lakf7ky.png align="left")

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/prw6vlhz1e8eubdpdws3.png align="left")

### **After updating the IAM role we need to restart the service and check the status.**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tyr5ysm1rfge7gqepdnj.png align="left")

### **Now go to the code deploy and create a deployment group.**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k46b4tf5a60c7cg53eje.png align="left")

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/opv8jycafti5rmteju3s.png align="left")

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/udzfdssxib4e7dl5owhq.png align="left")

### **Now we have to create the deployment, go to the application, and create a deployment.**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eha017zq7heqwi78hh1n.png align="left")

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1tlx2q38lvk5o8q1rx24.png align="left")

### **Copy the latest commit ID and paste it into the commit ID**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xpt6sh36g9vzvzvmb9i6.png align="left")

### **Click on Create Deployment now**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/72n5j3bdr05ffe9eauef.png align="left")

### **Build is successful**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1qhwya8niuudod4w4e6z.png align="left")

### **Now go to the code pipeline and edit tab** Go to the build stage and add a stage. Set the name as code-deploy then add an action group

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/unp2w7d4fri9gnoegleq.png align="left")

### **After done click on the save button**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/acdwww1b5ynoavm5xwwh.png align="left")

### **Finally, the application is running :)**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/aiyozzs5wy0hlid938rr.png align="left")

## Conclusion

Following this guide, you've successfully set up a CI/CD pipeline using AWS CodePipeline, CodeBuild, and CodeDeploy. This automated pipeline allows you to integrate, build, and deploy your application efficiently. As you refine your skills, consider adding additional stages for testing or integrating security checks.

***Happy automating!***