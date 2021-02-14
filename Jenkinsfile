  
pipeline {
environment {

KUBECONFIG = '~./kube/config'

imagename = "76896/afriprudential/aptest"
registryCredential = 'dockerHub'
configI = 'mykubeconfig'
dockerImage = ''
}


agent any
stages {
stage('Cloning Git') {
steps {
git([url: 'https://github.com/damyl4sure/aptest.git', branch: 'main'])
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
