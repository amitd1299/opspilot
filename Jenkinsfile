pipeline {
    agent any

    stages {
        stage('Environment Check') {
            steps {
                sh '''
                    echo "=== Environment ==="
                    java -version
                    docker --version
                    echo "=== Workspace ==="
                    pwd
                    ls -la
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Building OpsPilot backend'
                sh 'ls -la backend'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing OpsPilot backend'
                sh '''
                    docker build -t opspilot-backend:test ./backend
                    docker run -d --name opspilot-test -p 5001:5000 opspilot-backend:test
                    sleep 5
                    curl -f http://localhost:5001/health
                    docker rm -f opspilot-test
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building OpsPilot Docker image'
                sh 'docker build -t opspilot-backend:1.0 ./backend'
            }
        }
    }

    post {
        success {
            echo 'OpsPilot CI Pipeline completed successfully!'
        }
        failure {
            echo 'OpsPilot CI Pipeline failed.'
        }
        always {
            sh 'docker rm -f opspilot-test 2>/dev/null || true'
        }
    }
}
