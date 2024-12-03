# AWS Continuous Integration

## Set Up GitHub Repository

The first step in CI journey is to set up a GitHub repository to store our Python application's source code. If you already have a repository, feel free to skip this step. Otherwise, let's create a new repository on GitHub by following these steps:

- Go to github.com and sign in to your account.
- Click on the "+" button in the top-right corner and select "New repository."
- Give your repository a name and an optional description.
- Choose the appropriate visibility option based on your needs.
- Initialize the repository with a README file.
- Click on the "Create repository" button to create your new GitHub repository.

Great! Now that we have our repository set up, we can move on to the next step.

## Create an AWS CodePipeline
In this step, we'll create an AWS CodePipeline to automate the continuous integration process for our Python application. AWS CodePipeline will orchestrate the flow of changes from our GitHub repository to the deployment of our application. Let's go ahead and set it up:

- Go to the AWS Management Console and navigate to the AWS CodePipeline service.
- Click on the "Create pipeline" button.
- Provide a name for your pipeline and click on the "Next" button.
- For the source stage, select "GitHub" as the source provider.
- Connect your GitHub account to AWS CodePipeline and select your repository.
- Choose the branch you want to use for your pipeline.
- In the build stage, select "AWS CodeBuild" as the build provider.
- Create a new CodeBuild project by clicking on the "Create project" button.
- Configure the CodeBuild project with the necessary settings for your Python application, such as the build environment,  build commands, and artifacts.
- Save the CodeBuild project and go back to CodePipeline.
- Continue configuring the pipeline stages, such as deploying your application using AWS Elastic Beanstalk or any other suitable deployment option.
- Review the pipeline configuration and click on the "Create pipeline" button to create your AWS CodePipeline.

Awesome job! We now have our pipeline ready to roll. Let's move on to the next step to set up AWS CodeBuild.

## Configure AWS CodeBuild

In this step, we'll configure AWS CodeBuild to build our Python application based on the specifications we define. CodeBuild will take care of building and packaging our application for deployment. Follow these steps:

- In the AWS Management Console, navigate to the AWS CodeBuild service.
- Click on the "Create build project" button.
- Provide a name for your build project.
- For the source provider, choose "AWS CodePipeline."
- Select the pipeline you created in the previous step.
- Configure the build environment, such as the operating system, runtime, and compute resources required for your Python application.
- Specify the build commands, such as installing dependencies and running tests. Customize this based on your application's requirements.
- Set up the artifacts configuration to generate the build output required for deployment.
- Review the build project settings and click on the "Create build project" button to create your AWS CodeBuild project.


## Trigger the CI Process

In this final step, we'll trigger the CI process by making a change to our GitHub repository. Let's see how it works:

- Go to your GitHub repository and make a change to your Python application's source code. It could be a bug fix, a new feature, or any other change you want to introduce.
- Commit and push your changes to the branch configured in your AWS CodePipeline.
- Head over to the AWS CodePipeline console and navigate to your pipeline.
- You should see the pipeline automatically kick off as soon as it detects the changes in your repository.
- Sit back and relax while AWS CodePipeline takes care of the rest. It will fetch the latest code, trigger the build process with AWS CodeBuild, and deploy the application if you configured the deployment stage.



# AWS Continuous Deployment

## Overview

This project demonstrates a **Continuous Deployment (CD)** workflow using AWS services. Continuous Deployment ensures that all code changes passing automated tests are automatically deployed to the production environment, enabling rapid delivery of new features and updates.

## Prerequisites

Before starting, ensure you have:
- A completed CI pipeline that generates build artifacts.
- An AWS account with necessary IAM roles configured.
- An EC2 instance with CodeDeploy agent installed.

---

## Set Up AWS CodeDeploy

Follow these steps to set up AWS CodeDeploy:

1. **Create a Deployment Application**:
   - Navigate to the AWS CodeDeploy console.
   - Click "Create application."
   - Name the application and select the platform (e.g., EC2/On-Premises).
   
2. **Configure Deployment Groups**:
   - Define deployment groups under the application.
   - Specify the EC2 instances or Auto Scaling groups to deploy the application.

3. **Upload Application Artifacts**:
   - Package your application into a `.zip` or `.tar.gz` file.
   - Store the artifacts in an S3 bucket accessible by CodeDeploy.

4. **Define AppSpec File**:
   - Include an `appspec.yml` file in your package to define deployment steps and lifecycle hooks.
   - Example:
     ```yaml
     version: 0.0
     os: linux
     files:
       - source: /
         destination: /var/www/html
     hooks:
       AfterInstall:
         - location: scripts/after_install.sh
           timeout: 300
           runas: root
     ```

---

## Deploy to AWS EC2

1. **Install CodeDeploy Agent**:
   - SSH into the EC2 instance.
   - Run the following commands:
     ```bash
     sudo apt update
     sudo apt install ruby-full wget
     wget https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/latest/install
     chmod +x ./install
     sudo ./install auto
     sudo systemctl start codedeploy-agent
     ```

2. **Start Deployment**:
   - Go to CodeDeploy and create a deployment.
   - Choose the application, deployment group, and the artifact stored in S3.
   - Start the deployment and monitor its progress in the CodeDeploy console.

---

## Monitor Deployment

- **AWS CodeDeploy Console**: View deployment logs, errors, and lifecycle events.
- **CloudWatch Logs**: Analyze detailed logs of your application and deployment process.

---

## Feedback and Alerts

1. **Amazon SNS**:
   - Configure SNS to send notifications for deployment events.
   - Steps:
     - Create an SNS topic in the SNS console.
     - Subscribe to the topic (e.g., email or SMS).
     - Integrate the topic with CodeDeploy notification rules.

2. **Discord Alerts**:
   - Use webhooks to send alerts to a Discord channel.
   - Configure alerts for deployment success, failure, or threshold breaches.

3. **Monitoring with Grafana**:
   - Set up Grafana with Prometheus for real-time metrics.
   - Configure alerts for resource usage or application performance.

---

## References

- AWS Documentation: [CodeDeploy](https://docs.aws.amazon.com/codedeploy)
- Medium Article: [Building a CD Pipeline](https://medium.com)
- Tools: [Grafana](https://grafana.com), [Prometheus](https://prometheus.io)
