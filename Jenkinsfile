pipeline {
agent any
environment {
// define environment variable
// Jenkins credentials configuration
DOCKER_HUB_CREDENTIALS = credentials('dockerhub_credentials') // Docker Hub credentials ID store in Jenkins
// Docker Hub Repository's name
DOCKER_IMAGE = 'szyzbg/szyteedy' // your Docker Hub user name and Repository's name
DOCKER_TAG = "${env.BUILD_NUMBER}" // use build number as tag
}
stages {
stage('Start Minikube') {
steps {
sh '''
if ! minikube status | grep -q "Running"; then
echo "Starting Minikube..."
minikube start
else
echo "Minikube already running."
fi
'''
}
}
stage('Set Image') {
steps {
sh '''
echo "Setting image for deployment..."
// kubectl set image deployment/hello-node4 docs=sismics/docs:latest
'''
}
}
stage('Verify') {
steps {
sh 'kubectl rollout status deployment/hello-node4'
sh 'kubectl get pods'
}
}
}
}
