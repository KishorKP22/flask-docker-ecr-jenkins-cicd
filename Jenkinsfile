pipeline {
    agent any

    environment {
        ECR_REPO = '850995567327.dkr.ecr.ap-south-1.amazonaws.com/flask-app'
        IMAGE_TAG = "flask-app-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/KishorKP22/flask-docker-ecr-jenkins-cicd.git'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ap-south-1 | \
                docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh 'docker push $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop flask-container || true
                docker rm flask-container || true
                docker run -d -p 8000:8000 --name flask-container $ECR_REPO:$IMAGE_TAG
                '''
            }
        }
    }
}
