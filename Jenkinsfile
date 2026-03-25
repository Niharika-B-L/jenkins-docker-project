pipeline {
    agent any
    
    environment {
        DOCKER_HUB_USER = 'nihadocker'
        IMAGE_NAME = 'niharikabl995/jenkins-docker-project'
    }

    stages {
        stage('Clone Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use 'bat' instead of 'sh' for Windows
                    bat "docker build -t %DOCKER_HUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER% ."
                    bat "docker tag %DOCKER_HUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER% %DOCKER_HUB_USER%/%IMAGE_NAME%:latest"
                }
            }
        }

        stage('Login and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nihadocker', passwordVariable: 'DOCKER_PASS', usernameVariable: 'niharikabl995')]) {
                    // In Windows Batch, use %VAR% instead of $VAR
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                    bat "docker push %DOCKER_HUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER%"
                    bat "docker push %DOCKER_HUB_USER%/%IMAGE_NAME%:latest"
                }
            }
        }
    }
}
