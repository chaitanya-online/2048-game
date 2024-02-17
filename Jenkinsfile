pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_REGISTRY_CREDENTIALS = 'dockerauth'
    }
    stages {
        stage('Clone') {
            steps {
                script {
                    echo "Clone started"
                    gitInfo = checkout scm
                }
            }
        }

        stage('Docker build') {
            steps {
                script {
                    sh """
                        docker build -t richeb/2048-game .
                    """
                }
            }
        }

        stage('Image push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDENTIALS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh """
                        docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                        docker push richeb/2048-game
                        """
                    }
                }
            }
        }
    }
}