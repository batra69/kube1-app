pipeline {
    agent any

    stages {

        stage('Verify Docker PATH') {
            steps {
                sh '''
                export PATH=/usr/local/bin:/opt/homebrew/bin:$PATH
                echo "PATH=$PATH"
                which docker
                docker --version
                '''
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    export PATH=/usr/local/bin:/opt/homebrew/bin:$PATH
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                export PATH=/usr/local/bin:/opt/homebrew/bin:$PATH
                docker build -t sanchit69/cicdprac-app:latest .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                export PATH=/usr/local/bin:/opt/homebrew/bin:$PATH
                docker push sanchit69/cicdprac-app:latest
                '''
            }
        }
    }

    post {
        always {
            sh '''
            export PATH=/usr/local/bin:/opt/homebrew/bin:$PATH
            docker logout || true
            docker rmi sanchit69/cicdprac-app:latest || true
            docker image prune -f || true
            '''
        }
    }
}
