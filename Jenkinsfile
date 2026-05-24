pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mysite:v1 .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f mysite-container || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 80:80 --name mysite-container mysite:v1'
            }
        }
    }
}
