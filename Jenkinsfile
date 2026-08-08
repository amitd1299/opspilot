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
                echo 'OpsPilot build stage'
            }
        }

        stage('Test') {
            steps {
                echo 'OpsPilot test stage'
            }
        }

        stage('Docker') {
            steps {
                echo 'Docker build stage will be added next'
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
    }
}
