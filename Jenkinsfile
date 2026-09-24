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
                echo 'Running application tests...'
                sh '''
                    echo "Running basic application validation..."
                    test -f app/index.html
                    test -f app/style.css
                    test -f app/script.js
                    echo "Application files validated successfully."
                '''
            }
        }

    }
}
