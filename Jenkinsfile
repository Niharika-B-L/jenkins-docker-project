pipeline {
    agent any
    
    environment {
        // This matches the ID you created in Jenkins Credentials
        DOCKER_HUB_USER = 'your-docker-username'
        IMAGE_NAME = 'jenkins-docker-project'
    }

    stages {
        stage('Clone Code') {
            steps {
                checkout scm // Automatically pulls from the GitHub repo you link in the Job
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Builds the image and tags it
                    sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER} ."
                    sh "docker tag ${DOCKER_HUB_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER} ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Login and Push') {
            steps {
                // 'dockerhub-creds-id' must match the ID you set in Jenkins
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds-id', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment Successful! Check Docker Hub.'
        }
    }
}
