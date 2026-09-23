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
    }
}
