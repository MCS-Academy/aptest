  
pipeline {
environment {

KUBECONFIG = '~./kube/config'

imagename = "dammy1/afriprudentialtest/aptest"
registryCredential = 'dockerHub'
configI = 'mykubeconfig'
dockerImage = ''
}


agent any
stages {
stage('Cloning Git') {
steps {
git([url: 'https://github.com/damyl4sure/aptest.git', branch: 'dammy-patch-1'])
}
}


stage('Building image') {
steps{
script {
dockerImage = docker.build imagename
}
}
}


stage('Deploy Image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push("$BUILD_NUMBER")
dockerImage.push('latest')
}
}
}
}

stage('Remove Unused docker image') {
steps{
sh "docker rmi $imagename:$BUILD_NUMBER"
sh "docker rmi $imagename:latest"
}
}

stage('Deploy App') {
steps {
script {
kubernetesDeploy(configs: "deployment.yml", kubeconfigId: configI)
        }
      }
                   }
  
}
}