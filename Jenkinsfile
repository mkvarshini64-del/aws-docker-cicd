stage('Deploy to EC2') {
    steps {
        echo 'Deploying application to EC2...'

        withCredentials([
            sshUserPrivateKey(
                credentialsId: 'ec2-ssh-key1',
                keyFileVariable: 'SSH_KEY',
                usernameVariable: 'SSH_USER'
            )
        ]) {
            sh '''
                chmod 600 "$SSH_KEY"

                ssh -o StrictHostKeyChecking=no \
                    -i "$SSH_KEY" \
                    "$SSH_USER@ec2-98-81-158-238.compute-1.amazonaws.com" \
                    '
                        docker pull varshinimk/travel-app:v2 &&
                        docker stop travel-app || true
                        docker rm travel-app || true
                        docker run -d --name travel-app -p 80:80 varshinimk/travel-app:v2
                    '
            '''
        }
    }
}
