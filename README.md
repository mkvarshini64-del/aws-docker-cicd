# AWS Docker CI/CD Travel Application

## Project Overview

This project demonstrates an end-to-end CI/CD pipeline for a Dockerized travel web application.

The project integrates GitHub, Jenkins, Docker, Docker Hub, and AWS services to automate application build, container image creation, image publishing, and deployment to an AWS EC2 instance.

## Architecture

```
Developer
    |
    v
   Git
    |
    v
GitHub
    |
    v
Jenkins
    |
    +----> Test Application
    |
    +----> Build Docker Image
    |
    v
Docker Hub
    |
    v
AWS EC2
    |
    v
Docker Container
    |
    v
Application Load Balancer
    |
    v
Travel Explorer
```

**Technologies Used**

| Technology                | Purpose                      |
| ------------------------- | ---------------------------- |
| Git                       | Source code version control  |
| GitHub                    | Source code repository       |
| Jenkins                   | CI/CD automation             |
| Docker                    | Application containerization |
| Docker Hub                | Docker image registry        |
| AWS EC2                   | Application hosting          |
| Application Load Balancer | Traffic distribution         |
| Auto Scaling              | EC2 capacity management      |
| CloudWatch                | Monitoring                   |
| SNS                       | Notifications                |

**Application**

Travel Explorer is a simple web application containing:

- HTML
- CSS
- JavaScript

Application files:
```
app/
├── index.html
├── style.css
└── script.js
```
**Docker**

The application is packaged using Nginx Alpine.

Dockerfile:

FROM nginx:alpine

COPY app/ /usr/share/nginx/html/

EXPOSE 80



**Jenkins CI/CD Pipeline**

The Jenkins pipeline performs:

+ Checkout source code from GitHub
+ Validate application files
+ Build Docker image
+ Tag image using Jenkins BUILD_NUMBER
+ Push image to Docker Hub
+ Connect to EC2 using SSH
+ Pull the versioned image
+ Stop the existing container
+ Remove the existing container
+ Start the new container

**Docker Image Versioning**

The Jenkins build number is used as the Docker image tag.

Example:
```
Jenkins Build #32
        |
        v
travel-app:32
        |
        v
varshinimk/travel-app:32
        |
        v
     AWS EC2
```
This allows different application versions to be identified and retained.


**AWS Infrastructure**

**EC2**

Runs the Dockerized Travel Explorer application.

**Application Load Balancer**

Receives application traffic and forwards requests to the EC2 instance.

**Auto Scaling**

Provides the ability to adjust EC2 capacity based on the configured Auto Scaling settings.

**CloudWatch**

Monitors EC2 CPU utilization.

A CloudWatch alarm was configured for high CPU utilization.

**SNS**

Used with the CloudWatch alarm for notifications.


**Deployment Verification**

The deployed container can be checked on EC2 using:

docker ps

varshinimk/travel-app:32

**Rollback Strategy**

Versioned Docker images make rollback possible.

For example:

Current version:
varshinimk/travel-app:32

Previous version:
varshinimk/travel-app:v2

**Repository Structure**

```
aws-docker-cicd/
├── app/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── docs/
│   ├── architecture.md
│   └── deployment.md
├── Dockerfile
├── Jenkinsfile
├── .gitignore
└── README.md
```

**Key DevOps Concepts Practiced**
- Git branching
- GitHub
- Docker
- Docker image versioning
- Docker Hub
- Jenkins CI/CD
- Jenkins credentials
- SSH-based deployment
- AWS EC2
- Application Load Balancer
- Auto Scaling
- CloudWatch
- SNS
- CI/CD troubleshooting
- Deployment and rollback concepts

**Project Outcome**

The project demonstrates an automated workflow where source code changes can be processed through Jenkins, packaged as a versioned Docker image, pushed to Docker Hub and deployed to AWS EC2.
