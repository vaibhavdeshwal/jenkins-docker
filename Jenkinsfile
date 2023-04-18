// pipeline {
//   agent {
//     kubernetes {
//       cloud 'kubernetes'
//       label 'jenkins-agent'
//       yaml """
//         apiVersion: v1
//         kind: Pod
//         metadata:
//           labels:
//             jenkins: agent
//             jenkins/label: jenkins-agent
//         spec:
//           containers:
//           - name: docker
//             image: docker:latest
//             command:
//             - sleep
//             args:
//             - infinity
//           - name: kubectl
//             image: bitnami/kubectl:latest
//             command:
//             - sleep
//             args:
//             - infinity
//       """
//     }
//   }
//   environment {
//     DOCKER_IMAGE = "vaibhavdeshwal/simple-docker-app"
//   }
//   stages {
//     stage('Build Docker Image') {
//       steps {
//         script {
//           docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
//             def customImage = docker.build(env.DOCKER_IMAGE)
//           }
//         }
//       }
//     }
//     stage('Push Docker Image') {
//       steps {
//         script {
//           docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
//             docker.image(env.DOCKER_IMAGE).push()
//           }
//         }
//       }
//     }
//   }
// }


pipeline {
 agent {
    kubernetes {
      cloud 'kubernetes'
      label 'jenkins-agent'
      yaml """
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            jenkins: agent
            jenkins/label: jenkins-agent
        spec:
          containers:
          - name: docker
            image: busybox
            command:
            - sleep
            args:
            - infinity
          - name: kubectl
            image: busybox
            command:
            - sleep
            args:
            - infinity
      """
    }
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t vaibhavdeshwal/jenkins-docker-hub .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push vaibhavdeshwal/jenkins-docker-hub'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}