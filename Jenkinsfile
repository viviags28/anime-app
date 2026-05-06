pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/viviags28/anime-app.git'
            }
        }

        stage('Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin

                    docker build -t viviags/backend-anime:latest ./backend
                    docker build -t viviags/frontend-anime:latest -f ./frontend/Dockerfile ./frontend

                    docker push viviags/backend-anime:latest
                    docker push viviags/frontend-anime:latest
                    '''
                }
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