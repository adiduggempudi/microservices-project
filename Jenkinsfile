pipeline {
    agent any 
       stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t adinarayana25/adservice:latest ."
                    }
                }
            }
        }
        stage ('Scan Image') {
            steps {
                sh 'trivy image adinarayana25/adservice:latest '
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push adinarayana25/adservice:latest "
                    }
                }
            }
        }
    }
}
