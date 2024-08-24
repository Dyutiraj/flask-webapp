pipeline {
    agent any
    environment{
        IMAGE_REPO_NAME="flask-webapp"
        IMAGE_TAG="${env.BUILD_NUMBER}"
        REPOSITORY_URI ="024848458348.dkr.ecr.us-east-1.amazonaws.com/dyutiraj/webapp"
    }
    stages {
        stage ('Bulinding Image') {
            steps{
                script {
                    dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
