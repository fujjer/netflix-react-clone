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
                    docker.build("648762884360.dkr.ecr.me-central-1.amazonaws.com/netflix-jan:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 648762884360.dkr.ecr.me-central-1.amazonaws.com/netflix-jan:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://648762884360.dkr.ecr.me-central-1.amazonaws.com/netflix-jan', 'ecr:me-central-1:fujjer-ecr') {
                    // build image
                    def myImage = docker.build("648762884360.dkr.ecr.me-central-1.amazonaws.com/netflix-jan:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
