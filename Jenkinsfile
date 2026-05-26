pipeline {
    agent any
    
    environment {
        AWS_ACCOUNT_ID = "628799989830"
        AWS_DEFAULT_REGION = "us-east-1"
        REPOSITORY_URI = "628799989830.dkr.ecr.us-east-1.amazonaws.com/bookmyshow-app"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                dir('bookmyshow-app') {
                    sh "docker build -t ${REPOSITORY_URI}:latest ."
                }
            }
        }
        
        stage('Push to ECR') {
            steps {
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                sh "docker push ${REPOSITORY_URI}:latest"
            }
        }

        stage('Deploy to EKS (Ansible)') {
            steps {
                sh "ansible-playbook deploy.yml"
            }
        }
    }
}
