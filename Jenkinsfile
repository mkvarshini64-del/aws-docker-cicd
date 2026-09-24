pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Travel Explorer application...'

                sh '''
                    test -f app/index.html
                    test -f app/style.css
                    test -f app/script.js

                    echo "Application files validated successfully."
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo "Building Docker image version ${BUILD_NUMBER}..."

                sh '''
                    docker build \
                        --platform linux/amd64 \
                        -t travel-app:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Docker Push') {
            steps {
                echo "Logging in and pushing image version ${BUILD_NUMBER}..."

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                            -u "$DOCKER_USERNAME" \
                            --password-stdin

                        docker tag travel-app:${BUILD_NUMBER} \
                            $DOCKER_USERNAME/travel-app:${BUILD_NUMBER}

                        docker push \
                            $DOCKER_USERNAME/travel-app:${BUILD_NUMBER}

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo "Deploying application version ${BUILD_NUMBER} to EC2..."

                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'ec2-ssh-key1',
                        keyFileVariable: 'SSH_KEY',
                        usernameVariable: 'SSH_USER'
                    )
                ]) {

                    sh """
                        chmod 600 "\$SSH_KEY"

                        ssh -o StrictHostKeyChecking=no \
                            -i "\$SSH_KEY" \
                            "\$SSH_USER@ec2-98-81-158-238.compute-1.amazonaws.com" \
                            "
                                docker pull varshinimk/travel-app:${BUILD_NUMBER}

                                docker stop travel-app || true
                                docker rm travel-app || true

                                docker run -d \
                                    --name travel-app \
                                    -p 80:80 \
                                    varshinimk/travel-app:${BUILD_NUMBER}
                            "
                    """
                }
            }
        }
    }
}
