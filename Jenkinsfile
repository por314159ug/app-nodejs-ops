pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'nodejs-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/por314159ug/app-nodejs.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Clean Up Old Container') {
            steps {
                powershell '''
                $container = docker ps --filter "publish=3000" --format "{{.ID}}"
                if ($container) {
                    Write-Output "Stopping container: $container"
                    docker stop $container
                    docker rm $container
                } else {
                    Write-Output "No container found on port 3000"
                }
                '''
            }
        }
      
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("${env.DOCKER_IMAGE}:latest").run("-p 3000:3000")
                }
            }
        }
    }
}
