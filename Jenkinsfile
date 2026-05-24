pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/zishaandude/jenkin1',
                    branch: 'main'
                echo "Code checked out successfully"
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t static-website .'
                echo "Image built: static-website:latest"
            }
        }

        stage('Test') {
            steps {
                sh '''
                    # Run container temporarily for testing
                    docker run -d --name test-web -p 8082:80 static-website
                    sleep 3

                    # Health check
                    STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8082)
                    echo "HTTP Status: $STATUS"

                    # Stop test container
                    docker rm -f test-web

                    # Fail pipeline if not 200
                    [ "$STATUS" = "200" ] || exit 1
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f web || true
                    docker run -d --name web -p 80:80 static-website
                    echo "Container deployed on port 80"
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    sleep 2
                    STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:80)
                    echo "Live site HTTP Status: $STATUS"
                    [ "$STATUS" = "200" ] || exit 1
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS — site is live at http://<EC2-IP>:80"
        }
        failure {
            echo "Pipeline FAILED — check stage logs in Blue Ocean"
            sh 'docker rm -f test-web || true'
        }
    }
}
