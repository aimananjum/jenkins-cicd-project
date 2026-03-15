pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'aimananjum'
        IMAGE_NAME      = 'aimananjum/jenkins-flask-app'
    }

    stages {

        stage('1 - Checkout Code') {
            steps {
                echo '===> Pulling latest code from GitHub...'
                checkout scm
            }
        }

        stage('2 - Build Docker Image') {
            steps {
                echo '===> Building Docker image...'
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest"
                echo "===> Image built: ${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }

        stage('3 - Push to DockerHub') {
            steps {
                echo '===> Pushing image to DockerHub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                    sh "docker push ${IMAGE_NAME}:latest"
                }
                echo '===> Push complete!'
            }
        }

        stage('4 - Deploy Container') {
            steps {
                echo '===> Deploying application...'
                sh '''
                    docker stop flask-app || true
                    docker rm flask-app   || true
                    docker run -d \
                        --name flask-app \
                        --restart unless-stopped \
                        -p 5000:5000 \
                        aimananjum/jenkins-flask-app:latest
                    echo "===> App deployed on port 5000!"
                '''
            }
        }

    }

    post {
        success {
            echo "PIPELINE SUCCESS - Build #${BUILD_NUMBER} is live!"
        }
        failure {
            echo "PIPELINE FAILED - Check the logs above for errors"
        }
        always {
            sh 'docker logout || true'
        }
    }
}
