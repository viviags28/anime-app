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
        withCredentials([file(credentialsId: 'kube-config', variable: 'KUBECONFIG')]) {
            bat '''
            set KUBECONFIG=%KUBECONFIG%
            kubectl get nodes
            kubectl apply -f k8s.yaml
            '''
        }
    }
}

    }
}