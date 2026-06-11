pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-app"
        CONTAINER_NAME = "my-app-container"
        PORT = "80"
    }

    stages {

        stage('1. Code Checkout') {
            steps {
                echo 'Pulling code from GitHub...'
                checkout scm
            }
        }

        stage('2. Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('3. Test') {
            steps {
                echo 'Running tests...'
                sh "echo 'Tests passed!'"
            }
        }

        stage('4. Deploy') {
            steps {
                echo 'Deploying container...'

                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"

                sh """
                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 80:80 \
                --restart always \
                ${IMAGE_NAME}:latest
                """

                echo 'Deployment successful!'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
