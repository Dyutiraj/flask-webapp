pipeline {
    agent any
    environment{
        IMAGE_REPO_NAME="flask-webapp"
        IMAGE_TAG="${env.BUILD_NUMBER}"
        REPOSITORY_URI ="024848458348.dkr.ecr.us-east-1.amazonaws.com/dyutiraj/webapp"
    }
    stages {
        stage('Logging into AWS ECR'){
            steps {
                script {
                    sh """aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 024848458348.dkr.ecr.us-east-1.amazonaws.com"""
                }
            }
        }
        stage ('Bulinding Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_REPO_NAME}:${IMAGE_TAG}")
                }
            }
        }
        stage ('Pushing to ECR'){
            steps{
                script{
                    sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                    sh """docker push ${REPOSITORY_URI}:${IMAGE_TAG}"""
                }
            }
        }
    }
}
