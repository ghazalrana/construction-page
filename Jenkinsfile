pipeline {
   agent any
     environment{
       DOCKER_TAG = getDockerTag()
 }
   stages{
        stage('Build Docker Image'){
       steps{
       sh "docker build . -t gzlkhan/constructionapp:${DOCKER_TAG}"
          }
         }
      
      stage('Dockerhub Push'){
            
             steps{
              withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u gzlkhan -p ${dockerHubPwd}"
                    sh "docker push gzlkhan/constructionapp:${DOCKER_TAG}"
              }
           }
       }
      
      stage('Apply Kubernetes files') {
         steps{
    withKubeConfig([credentialsId: 'mykubeconfig', serverUrl: 'https://34.72.253.103']) {
      sh "kubectl create -f deployment.yaml"
    }
  }
}
      
      
      }
      
  }





def getDockerTag(){
def tag = sh script: 'git rev-parse HEAD', returnStdout: true
return tag
}
