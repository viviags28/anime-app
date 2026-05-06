pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/viviags28/anime-app.git'
            }
        }

        stage('Build & Push') {
            steps {
                sh '''
                docker build -t viviags/backend-anime .
                docker build -t viviags/frontend-anime -f Dockerfile.frontend .

                docker push viviags/backend-anime
                docker push viviags/frontend-anime
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