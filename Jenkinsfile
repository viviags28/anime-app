pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/viviags28/anime-app.git'
            }
        }

        stage('Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-login', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat '''
                    echo %PASSWORD% | docker login -u %USERNAME% --password-stdin

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
                bat '''
                az login --only-show-errors
                az aks get-credentials --resource-group anime-rg --name anime-cluster --overwrite-existing

                kubectl config current-context
                kubectl apply -f k8s.yaml --validate=false
                '''
            }
        }

    }
}