# Project Architecture

## Overview

This project demonstrates an end-to-end Docker CI/CD pipeline for a travel web application using GitHub, Jenkins, Docker, Docker Hub, and AWS.

## Architecture

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
Travel Explorer Application

## AWS Components

### EC2

Runs the Dockerized Travel Explorer application.

### Application Load Balancer

Receives HTTP traffic and forwards requests to the EC2 application.

### Auto Scaling

Provides the ability to automatically adjust the number of EC2 instances based on configured capacity and monitoring.

### CloudWatch

Used to monitor EC2 CPU utilization and trigger an alarm when CPU utilization exceeds the configured threshold.

### SNS

Used with the CloudWatch alarm for notifications.

## CI/CD Components

### GitHub

Stores the application source code and Jenkinsfile.

### Jenkins

Automates:

1. Source code checkout
2. Application validation
3. Docker image build
4. Docker Hub push
5. EC2 deployment

### Docker

Packages the Travel Explorer application into a container.

### Docker Hub

Stores versioned Docker images.

## Image Versioning

Docker images are tagged using the Jenkins build number.

Example:

Build #32

    travel-app:32

    varshinimk/travel-app:32

The same image version is deployed to EC2.
