pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-id')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/PonnuthaiSethuguru/devops-project.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ponnuthaisethuguru/devops-app:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
                sh 'docker push ponnuthaisethuguru/devops-app:latest'
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-key']) {
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@13.222.139.32/ "docker pull ponnuthaisethuguru/devops-app:latest && docker stop devops-app || true && docker rm devops-app || true && docker run -d -p 80:3001 --name devops-app ponnuthaisethuguru/devops-app:latest"'
                }
            }
        }
    }
}
