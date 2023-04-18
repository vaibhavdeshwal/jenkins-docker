pipeline {
  agent {
    kubernetes {
      cloud 'kubernetes'
    //   label 'jenkins-agent'
    //   yaml """
    //     apiVersion: v1
    //     kind: Pod
    //     metadata:
    //       labels:
    //         jenkins: agent
    //         jenkins/label: jenkins-agent
    //     spec:
    //       containers:
    //       - name: docker
    //         image: docker:latest
    //         command:
    //         - sleep
    //         args:
    //         - infinity
    //       - name: kubectl
    //         image: bitnami/kubectl:latest
    //         command:
    //         - sleep
    //         args:
    //         - infinity
    //   """
    }
  }
  environment {
    DOCKER_IMAGE = "vaibhavdeshwal/simple-docker-app"
  }
  stages {
    stage('Build Docker Image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            def customImage = docker.build(env.DOCKER_IMAGE)
          }
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            docker.image(env.DOCKER_IMAGE).push()
          }
        }
      }
    }
  }
}
