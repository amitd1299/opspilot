        stage('Nexus Docker Push') {
            steps {
                echo 'Pushing Docker image to Nexus'
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-docker-creds',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh '''
                        echo "$NEXUS_PASS" | docker login 13.207.189.235:8084 -u "$NEXUS_USER" --password-stdin
                        docker tag opspilot-backend:1.0 13.207.189.235:8084/opspilot-backend:1.0
                        docker push 13.207.189.235:8084/opspilot-backend:1.0
                        docker logout 13.207.189.235:8084
                    '''
                }
            }
        }
