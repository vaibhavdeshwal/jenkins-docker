pipeline {
environment {
registry = "vaibhav/deshwal"
registryCredential = 'dockerhub'
dockerImage = ''
}
agent {
    kubernetes {
        cloud 'kubernetes'
    }
}
stages {
stage('Cloning our Git') {
steps {
git 'https://github.com/vaibhavdeshwal/jenkins-docker'
}
}
stage('Building our image') {
steps{
script {
dockerImage = docker.build registry + ":$BUILD_NUMBER"
}
}
}
stage('Deploy our image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
}
}
}
}
stage('Cleaning up') {
steps{
sh "docker rmi $registry:$BUILD_NUMBER"
}
}
}
}