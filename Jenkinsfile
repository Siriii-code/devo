docker build -t %DOCKER_IMAGE%:v1 .
docker push %DOCKER_IMAGE%:latest
Docker treats v1 and latest as different tags, so the push fails.

Corrected Jenkinsfile
Since you are already using v1, the simplest fix is to push v1 as well.

groovy

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'siriisureshh/lab1'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Siriii-code/devo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:v1 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-PAT',
                    variable: 'DOCKER_PAT'
                )]) {
                    bat '''
                        @echo off
                        echo %DOCKER_PAT%| docker login -u siriisureshh --password-stdin
                        if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:v1'
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
