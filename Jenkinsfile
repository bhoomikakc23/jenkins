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

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry(
                        'https://index.docker.io/v1/',
                        'dockerhub-creds'
                    ) {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
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
