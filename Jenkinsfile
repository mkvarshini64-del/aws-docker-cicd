pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Travel Explorer application...'
                sh 'ls -la'
                sh 'ls -la app'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t travel-app:v2 .'
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Logging in and pushing image to Docker Hub...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker tag travel-app:v2 $DOCKER_USERNAME/travel-app:v2
                        docker push $DOCKER_USERNAME/travel-app:v2
                        docker logout
                    '''
                }
            }
        }
    }
}
