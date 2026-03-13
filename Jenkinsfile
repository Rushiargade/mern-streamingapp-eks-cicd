pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Rushiargade/mern-streamingapp-eks-cicd.git'
            }
        }

        stage('Build Backend Images') {
            steps {
                sh '''
                cd backend
                docker build -t auth-service -f authService/Dockerfile .
                docker build -t streaming-service -f streamingService/Dockerfile .
                docker build -t admin-service -f adminService/Dockerfile .
                docker build -t chat-service -f chatService/Dockerfile .
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                cd frontend
                docker build -t frontend-service .
                '''
            }
        }

        stage('Verify Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}