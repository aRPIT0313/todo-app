pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/aRPIT0313/todo-app.git'
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
                bat 'docker rm -f todo-container || exit 0'
            }
        }
        stage('Run Docker Container') {
            steps {
                bat 'docker run -d --name todo-container -p 5000:5000 todo-app:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl config use-context minikube'
                bat 'kubectl apply -f k8s-deployment.yaml --validate=false'
                bat 'kubectl rollout status deployment/todo-app'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'pip install pytest pytest-flask'
                bat 'pytest tests/test_unit.py -v --junitxml=results/unit.xml'
            }
        }
    }
    post {
        success { echo 'Deployed Successfully!' }
        failure  { echo 'Build Failed. Check logs.' }
    }
}