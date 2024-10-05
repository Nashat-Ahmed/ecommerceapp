pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-repo'
        KUBECONFIG = credentials('kubeconfig-id') 
    }
    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                    // Build Backend
                    dir('ecommerce-backend') {
                        sh 'docker build -t nashaat111/backend:latest .'
                    }
                    
                    // Build Frontend
                    dir('ecommerce-frontend') {
                        sh 'docker build -t nashaat111/frontend:latest .'
                    }
                    
                    // Push to DockerHub
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                        sh 'docker push nashaat111/backend:latest'
                        sh 'docker push nashaat111/frontend:latest'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes manifests
                    sh 'kubectl apply -f k8s/db-deployment.yml'
                    sh 'kubectl apply -f k8s/db-service.yml'
                    sh 'kubectl apply -f k8s/backend-deployment.yml'
                    sh 'kubectl apply -f k8s/backend-service.yml'
                    sh 'kubectl apply -f k8s/frontend-deployment.yml'
                    sh 'kubectl apply -f k8s/frontend-service.yml'
                }
            }
        }
    }
}
