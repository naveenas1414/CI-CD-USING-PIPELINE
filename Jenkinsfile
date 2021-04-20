pipeline {
agent any


stages {
stage('checkout') {
steps {

git branch: 'master', url: 'https://github.com/naveenas1414/CI-CD-using-Docker.git'

}
}
stage('Execute Maven') {
steps {

sh 'mvn package'
}
}


stage('Docker Build and Tag') {
steps {

sh 'docker build -t samplewebapp:latest .'
sh 'docker tag samplewebapp naveenkumara14/test:$BUILD_NUMBER'
//sh 'docker tag samplewebapp naveenkumara14/test:$BUILD_NUMBER'

}
}

stage('Publish image to Docker Hub') {

steps {
withDockerRegistry([ credentialsId: "naveenkumara14", url: "" ]) {

sh 'docker push naveenkumara14/test:$BUILD_NUMBER'
// sh 'docker push naveenkumara14/test:$BUILD_NUMBER'
}

}
}

stage('Run Docker container on Jenkins Agent') {

steps
{
sh "docker ps -f name=tomcat_test -q | xargs --no-run-if-empty docker container stop"
sh "docker ps -a -f status=exited -q | xargs --no-run-if-empty docker rm"
sh "docker run -it -d -p 8001:8080 --name tomcat_test naveenkumara14/test:$BUILD_NUMBER"

}
}

}
}
