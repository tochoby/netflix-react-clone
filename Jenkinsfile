pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("557528111268.dkr.ecr.us-east-2.amazonaws.com/netflix-chia:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 557528111268.dkr.ecr.us-east-2.amazonaws.com/netflix-chia:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://557528111268.dkr.ecr.us-east-2.amazonaws.com/netflix-chia', 'ecr:us-east-2:chia-ecr') {
                    // build image
                    def myImage = docker.build("557528111268.dkr.ecr.us-east-2.amazonaws.com/netflix-chia:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
