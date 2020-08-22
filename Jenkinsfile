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
   withCredentials([usernamePassword(credentialsId: 'clusterauth', passwordVariable: 'password', usernameVariable: 'Username')]) {
    
   sh "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project wordpress-learning-277315"
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
