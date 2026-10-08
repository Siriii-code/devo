pipeline{
agent any

environment{
DOCKER_IMAGE = "siriisureshh/lab1-image"
}

stages {
stage('Clone Repository') {
steps {
git 'https://github.com/Siriii-code/devo.git'
}
}
stage('Build Docker Image'){
steps{
script{
docker.build("${DOCKER_IMAGE}:v1)
}
}
}

stage('login to Docker hub') {
steps{
  withCredentials([usernamePassword(
credentialsId: 'dockerhub-cred',
usernameVariable: 'DOCKER_USER',
passwordVariable: 'DOCKER_PASS'
)]) {
bat 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
}
}
}

stage('Push Docker Image') {
steps {
script {
docker.	WithRegistry('','dockerhub-creds') {
docker.image("${DOCKER_IMAGE}:v1").push()
}
}
}
}
}

post {
success {
echo 'Image successfully built and pushed to Docker hub'
}
failure {
echo 'Pipeline failed'
}
}
}
