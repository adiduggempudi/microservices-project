pipeline {
    agent any
    environment {
        SCANNER_HOME= tool "mysonar"
    }
    stages {
        stage ('Code Quality Analysis') {
            steps {
               withSonarQubeEnv('mysonar') {
                     sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=myproject \
                      -Dsonar.projectKey=myproject '''
                       }
                   }
            }
        }
        stage ("Quality Gates") {
            steps {
                script {
                   waitForQualityGate abortPipeline: false, credentialsId: 'tokem'
                  }
            }
        }
  
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
