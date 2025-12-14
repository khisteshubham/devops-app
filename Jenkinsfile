pipeline {
    agent any

    environment {
        ECR_REPO = "515966531943.dkr.ecr.ap-south-1.amazonaws.com/devops-app"
        AWS_REGION = "ap-south-1"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/khisteshubham/devops-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t devops-app .'
            }
        }

        stage('Login to ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                    bat '''
                    aws ecr get-login-password --region %AWS_REGION% ^
                    | docker login --username AWS --password-stdin %ECR_REPO%
                    '''
                }
            }
        }

        stage('Tag & Push Image') {
            steps {
                bat '''
                docker tag devops-app:latest %ECR_REPO%:latest
                docker push %ECR_REPO%:latest
                '''
            }
        }
    }
}