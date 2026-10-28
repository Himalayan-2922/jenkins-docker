pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t web-demo:latest .'
            }
        }
        stage('Deploy') {
            steps {
                bat 'docker rm -f web-demo || exit /b 0'
                bat 'docker run -d --name web-demo -p 8090:80 web-demo:latest'
            }
        }
    }
}