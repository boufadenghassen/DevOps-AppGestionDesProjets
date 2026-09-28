pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'boufaden2'
        BACKEND_IMAGE  = "${DOCKERHUB_USER}/gestion-projets-backend"
        FRONTEND_IMAGE = "${DOCKERHUB_USER}/gestion-projets-frontend"
        TAG            = "${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/boufadenghassen/DevOps-AppGestionDesProjets.git'
            }
        }
        stage('Build Images') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE:$TAG -t $BACKEND_IMAGE:latest ./backend'
                sh 'docker build -t $FRONTEND_IMAGE:$TAG -t $FRONTEND_IMAGE:latest ./frontend'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                  usernameVariable: 'DH_USER',
                                                  passwordVariable: 'DH_TOKEN')]) {
                    sh 'echo "$DH_TOKEN" | docker login -u "$DH_USER" --password-stdin'
                    sh 'docker push $BACKEND_IMAGE:$TAG'
                    sh 'docker push $BACKEND_IMAGE:latest'
                    sh 'docker push $FRONTEND_IMAGE:$TAG'
                    sh 'docker push $FRONTEND_IMAGE:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose up -d --build'
            }
        }
    }
    post {
        always {
            sh 'docker logout || true'
        }
    }
}
