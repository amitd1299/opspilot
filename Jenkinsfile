pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

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
                    docker rm -f opspilot-test 2>/dev/null || true
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

        stage('Kubernetes Deploy') {
            steps {
                echo 'Deploying OpsPilot application to Kubernetes'
                sh '''
                    kubectl apply -f k8s/opspilot.yaml
                    kubectl rollout status deployment/opspilot-backend --timeout=120s
                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Verifying Kubernetes deployment'
                sh '''
                    kubectl get pods -o wide
                    kubectl get svc opspilot-backend
                    kubectl get deployment opspilot-backend
                    curl -f http://localhost:30278/health
                    echo "=== Kubernetes deployment successful ==="
                '''
            }
        }
    }

    post {
        success {
            echo 'OpsPilot CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'OpsPilot CI/CD Pipeline failed.'
        }

        always {
            sh 'docker rm -f opspilot-test 2>/dev/null || true'
        }
    }
}
