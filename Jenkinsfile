
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "bhoomikakc23/image"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                        powershell -NoProfile -NonInteractive -Command "$env:DOCKER_PASS | docker login -u $env:DOCKER_USER --password-stdin"
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:latest").push()
                }
            }
        }
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output for errors.'
        }

        always {
            echo 'CI/CD pipeline execution completed.'
        }
    }
}
