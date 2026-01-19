pipeline {
    agent any

    environment {
        IMAGE_NAME = "cicdprac-app"
        DOCKERHUB_USER = "sanchit69"
        DOCKER_CREDS = credentials('dockerhub-creds')
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                bat """
                echo %DOCKER_CREDS_PSW% | docker login -u %DOCKER_CREDS_USR% --password-stdin
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:latest .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                bat """
                docker push %DOCKERHUB_USER%/%IMAGE_NAME%:latest
                """
            }
        }
    }

    post {
        always {
            echo 'Cleaning Docker resources...'
            bat """
            docker logout
            docker rmi %DOCKERHUB_USER%/%IMAGE_NAME%:latest || exit 0
            docker image prune -f
            """
        }
    }
}
