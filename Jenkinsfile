pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/todo-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t todo-app:latest .'
            }
        }
        stage('Stop Old Container') {
            steps {
                bat 'docker stop todo-container || exit 0'
                bat 'docker rm todo-container || exit 0'
            }
        }
        stage('Run Docker Container') {
            steps {
                bat 'docker run -d --name todo-container -p 5000:5000 todo-app:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s-deployment.yaml'
                bat 'kubectl rollout status deployment/todo-app'
            }
        }
    }
    post {
        success { echo 'Deployed Successfully!' }
        failure  { echo 'Build Failed. Check logs.' }
    }
}