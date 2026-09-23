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
                sh 'docker build -t travel-app:v1 .'
            }
        }
    }
}
