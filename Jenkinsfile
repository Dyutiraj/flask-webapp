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
        stage ('Deploy to EC2'){
            steps{
                script{
                    echo 'Deploying docker image to EC2'
                    def dockerCmd = "docker run -p 5000:5000 -d ${REPOSITORY_URI}:${IMAGE_TAG}"
                sshagent(['64bfa5a8-810a-49fb-b212-634786144456']) {
                    sh "ssh -o StrictHostKeyChecking=no jenkins@100.27.26.141 ${dockerCmd}"
                }
            }
        }
    }
}
