# Deployment Guide

## CI/CD Flow

The Jenkins pipeline performs the following steps:

1. Checkout source code from GitHub
2. Validate application files
3. Build the Docker image
4. Tag the image using the Jenkins build number
5. Push the image to Docker Hub
6. Connect to AWS EC2 using SSH
7. Pull the versioned Docker image
8. Stop the existing application container
9. Remove the existing container
10. Start the new application container

## Docker Image Versioning

The Jenkins build number is used as the Docker image tag.

Example:

    Build #32

Docker image:

    travel-app:32

Docker Hub image:

    varshinimk/travel-app:32

The same version is deployed to EC2.

## EC2 Deployment

The application runs inside a Docker container.

Container configuration:

    Container name: travel-app
    Application port: 80
    Host port: 80

Example:

    0.0.0.0:80->80/tcp

## Deployment Verification

After deployment, the running container can be verified using:

    docker ps

The deployed image can be verified using:

    docker images | grep travel-app

Example:

    varshinimk/travel-app:32

## Rollback Strategy

Previous Docker image versions can be retained on Docker Hub and EC2.

If a newly deployed version needs to be rolled back, a previously working image version can be deployed.

Example:

    varshinimk/travel-app:32

can be replaced with a previously available version.

## AWS Cost Management

When the project is not being used, AWS resources can be scaled down or terminated to avoid unnecessary running-resource costs.

The Launch Template can be retained so the infrastructure can be recreated later.
