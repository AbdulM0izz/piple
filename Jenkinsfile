pipeline {
    agent any

    environment {
        CONTAINER_NAME = "nestjs-name"
        IMAGE_NAME = "nestjs-image"
        EMAIL = "abdulmoiz95972@gmail.com"
        PORT = "3000"
    }

    stages {
        stage('Code Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/AbdulM0izz/piple.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🛠️ Building Docker image..."
                    retry(2) {
                        sh 'docker build -t ${IMAGE_NAME} .'
                    }
                }
            }
        }

        stage('Remove Previous Container') {
            steps {
                script {
                    echo "🧹 Removing previous container (if any)..."
                    sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "🚀 Running Docker container..."
                    sh '''
                    docker run -d -p ${PORT}:${PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}
                    '''
                }
            }
        }

        stage('Send Email') {
            steps {
                emailext (
                    subject: "NestJS App Deployed",
                    body: "✅ The application has been deployed successfully at http://16.170.220.97:${PORT}/",
                    to: "${EMAIL}"
                )
            }
        }
    }

    post {
        failure {
            emailext (
                subject: "❌ NestJS Deployment Failed",
                body: "⚠️ The Jenkins pipeline for NestJS failed. Please check the logs.",
                to: "${EMAIL}"
            )
        }
    }
}
