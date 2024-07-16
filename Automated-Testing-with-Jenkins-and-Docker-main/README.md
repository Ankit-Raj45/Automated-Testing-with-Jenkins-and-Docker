# Automated Testing with Jenkins and Docker

This repository demonstrates a Jenkins pipeline implementation for achieving continuous integration and continuous deployment (CI/CD) using Docker and GitHub.

## Introduction

Automated testing with Jenkins and Docker optimizes software development workflows by seamlessly integrating continuous integration and deployment. Jenkins orchestrates the automated testing process, while Docker ensures consistent and isolated testing environments. This combination allows for rapid provisioning of testing environments, facilitates parallel testing, and enhances overall software quality. By automating the setup and execution of tests within Docker containers managed by Jenkins, teams achieve faster feedback cycles and improved reliability in their software releases.

## Prerequisites

Before setting up the pipeline, ensure that you have the following prerequisites:

1. An Amazon EC2 Linux machine with Jenkins, Git, and Docker installed.
2. A GitHub repository containing your application code and Dockerfile.

## Project Pipeline Flowchart

The CI/CD pipeline workflow is represented as follows:

![Jenkins CI_CD Pipeline with Docker and GitHub](https://i0.wp.com/collabnix.com/wp-content/uploads/2018/03/ci-cd.png?w=680&ssl=1)


## Installation Instructions

### Installing Jenkins

To install Jenkins, follow these steps:

1. Ensure that your software packages are up to date on your instance by using the following command to perform a quick software update:
    ```bash
    sudo yum update –y
    ```

2. Add the Jenkins repository using the following command:
    ```bash
    sudo wget -O /etc/yum.repos.d/jenkins.repo \
        https://pkg.jenkins.io/redhat-stable/jenkins.repo
    ```

3. Import a key file from Jenkins-CI to enable installation from the package:
    ```bash
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
    ```

4. Upgrade the system packages:
    ```bash
    sudo yum upgrade
    ```

5. Install Java (Amazon Linux 2):
    ```bash
    sudo amazon-linux-extras install java-openjdk11 -y
    ```

   Install Java (Amazon Linux 2023):
    ```bash
    sudo dnf install java-11-amazon-corretto -y
    ```

6. Install Jenkins:
    ```bash
    sudo yum install jenkins -y
    ```

7. Enable the Jenkins service to start at boot:
    ```bash
    sudo systemctl enable jenkins
    ```

8. Start Jenkins as a service:
    ```bash
    sudo systemctl start jenkins
    ```

9. You can check the status of the Jenkins service using the command:
    ```bash
    sudo systemctl status jenkins
    ```
### Installing Docker

To install Docker, follow these steps:

1. Install Docker:
    ```bash
    sudo yum install docker -y
    ```

2. Enable Docker to start on system boot:
    ```bash
    sudo systemctl enable docker
    ```

3. Start the Docker service:
    ```bash
    sudo systemctl start docker
    ```

### Installing Git

To install Git, follow these steps:

1. Install Git:
    ```bash
    sudo yum install git -y
    ```

### Additional Configuration

To allow Jenkins to interact with Docker, execute the following command:

```bash
sudo usermod -aG docker jenkins
```
After executing the above command, restart Jenkins:

```bash
sudo systemctl restart jenkins
```

## Pipeline Overview

A pipeline in software development refers to a series of automated steps that outline the continuous integration and delivery (CI/CD) process. It typically encompasses building, testing, and deploying code changes.

In CI/CD pipelines, stages are executed sequentially, ensuring code quality through automated testing and maintaining consistency across environments with tools like Jenkins, GitLab CI/CD, or GitHub Actions. Pipelines promote collaboration and efficiency by automating repetitive tasks and providing rapid feedback on code changes.

Key components include defining stages (e.g., build, test, deploy), specifying triggers (e.g., commits to specific branches), integrating with version control systems for source code management, and incorporating automated testing frameworks (e.g., JUnit, Selenium).

Pipelines enhance software development by reducing manual errors, accelerating time-to-market, and fostering a culture of continuous improvement through iterative development cycles. They are integral to modern software development practices, enabling teams to deliver reliable and high-quality software more efficiently.

## Getting Started

To get started with this CI/CD pipeline, follow the steps below:

1. Set up Jenkins, Git, and Docker on your machine.
2. Provide Jenkins with a GitHub credential (token):
   - Generate a GitHub personal access token with the appropriate scopes (repo, webhook, etc.).
   - In Jenkins, go to "Manage Jenkins" > "Manage Credentials"> "Global credentials (unrestricted)".
   - Add a new credential of type "Secret text" or "Secret file" and enter your GitHub token.
   - Save the credential. 
3. Configure Jenkins by accessing its web interface.
4. Create a new Jenkins job and configure it as follows:
   - Set the job type to "Freestyle Project".
   - Connect it to your GitHub repository (https://github.com/kamleshrawat/Automated-Testing-with-Jenkins-and-Docker.git) and configure the webhook.
   - Select "GitHub hook trigger for GITScm polling" as the build trigger.
   - Add an "Execute Shell" build step to the pipeline and use the following code:
   ```bash
   #!/bin/bash

    container_id=$(docker ps --filter "status=running" --format "{{.ID}}")

    if [ -n "$container_id" ]; then
    docker cp /var/lib/jenkins/workspace/devops-project/. "$container_id":/usr/share/nginx/html
    else
    docker build -t server /var/lib/jenkins/workspace/devops-project
    docker run -d -p 9090:80 server
    fi
    ```
Run the Jenkins job and verify the successful execution of the pipeline.


![Screenshot (89)](https://github.com/user-attachments/assets/4857453b-1463-4a3c-85d5-add02d6a5e9c)


*Application is running, and whenever a developer commits changes to the GitHub repository, it will automatically get deployed to the application.*

