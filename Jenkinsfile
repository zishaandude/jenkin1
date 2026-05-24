pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/zishaandude/jenkin1.git',
                    branch: 'main'
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
}
