pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Code checked out"
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t static-website .'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f web || true
                    docker run -d --name web -p 80:80 static-website
                '''
            }
        }
    }

    post {
        success { echo "Site live at http://<EC2-IP>:80" }
        failure { echo "Check logs above" }
    }
}
