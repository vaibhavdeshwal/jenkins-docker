pipeline {
    agent {
        kubernetes {
            cloud 'kubernetes'
            label 'myslavepod'
        }
    }
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKER_IMAGE_NAME = 'myapp'
        DOCKER_IMAGE_TAG = 'latest'
        DOCKER_REPO_URL = 'vaibhavdeshwal/myapp'
        GITHUB_REPO_URL = 'https://github.com/vaibhavdeshwal/jenkins-docker'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: GITHUB_REPO_URL]]])
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        dockerImage.push("${DOCKER_REPO_URL}:${DOCKER_IMAGE_TAG}")
                    }
                }
            }
        }
    }
}
