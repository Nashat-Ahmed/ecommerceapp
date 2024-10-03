pipeline {
    agent any
    stages {
        stage('Build Backend') {
            steps {
                dir('ecommerce-backend') {
                    sh 'docker build -t nashaat111/backend:latest .'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('ecommerce-frontend') {
                    sh 'docker build -t nashaat111/frontend:latest .'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                        sh 'docker push nashaat111/backend:latest'
                        sh 'docker push nashaat111/frontend:latest'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f db-deployment.yml'
                sh 'kubectl apply -f db-service.yml'
                sh 'kubectl apply -f backend-deployment.yml'
                sh 'kubectl apply -f backend-service.yml'
                sh 'kubectl apply -f frontend-deployment.yml'
                sh 'kubectl apply -f frontend-service.yml'
            }
        }
    }
}

