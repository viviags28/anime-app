pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/username/repo-kamu.git'
            }
        }

        stage('Build & Push') {
            steps {
                sh '''
                docker build -t username/backend-anime .
                docker build -t username/frontend-anime -f Dockerfile.frontend .

                docker push username/backend-anime
                docker push username/frontend-anime
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f k8s.yaml
                '''
            }
        }
    }
}