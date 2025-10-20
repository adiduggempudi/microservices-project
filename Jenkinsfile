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
        stage ("Quality Gates") {
            steps {
                script {
                   waitForQualityGate abortPipeline: false, credentialsId: 'tokem'
                  }
            }
        }
  
    stages {
        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-1', contextName: '', credentialsId: 'k8s', namespace: 'webapps', serverUrl: 'https://47D73E72934DC3CD63C80C2D82053ADD.yl4.ap-south-1.eks.amazonaws.com']]) {
                    sh "kubectl apply -f deployment-service.yml"
                    
                }
            }
        }
        
        stage('verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-1', contextName: '', credentialsId: 'k8s', namespace: 'webapps', serverUrl: 'https://47D73E72934DC3CD63C80C2D82053ADD.yl4.ap-south-1.eks.amazonaws.com']]) {
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}
