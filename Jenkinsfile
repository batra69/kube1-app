pipeline {
    agent any

    environment {
        IMAGE_NAME = "cicdprac-app"
        DOCKERHUB_USER = "sanchit69"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t sanchit69/cicdprac-app:latest .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker push sanchit69/cicdprac-app:latest
                '''
            }
        }
    }

    post {
        always {
            sh '''
            docker logout || true
            docker rmi sanchit69/cicdprac-app:latest || true
            docker image prune -f || true
            '''
        }
    }
}
