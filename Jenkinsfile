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
                    sshagent(['cc6900e9-079c-4a61-becf-3862b5dab61a']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@100.27.26.141 << EOF
                        docker pull ${REPOSITORY_URI}:${IMAGE_TAG}
                        docker run -d --name your-container-name -p 5000:5000 ${REPOSITORY_URI}:${IMAGE_TAG}
                        EOF
                        """
                }
            }
        }
    }
}
}
