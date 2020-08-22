pipeline {
   agent any
     environment{
       DOCKER_TAG = getDockerTag()
        PROJECT_ID = 'wordpress-learning-277315'
        CLUSTER_NAME = 'cluster-1'
        LOCATION = 'us-central1-c'
        CREDENTIALS_ID = 'Wordpress-Learning'
 }
   stages{
        stage('Build Docker Image'){
       steps{
       sh "docker build . -t gzlkhan/constructionapp:${env.BUILD_ID}"
          }
         }
      
      stage('Dockerhub Push'){
            
             steps{
              withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u gzlkhan -p ${dockerHubPwd}"
                    sh "docker push gzlkhan/constructionapp:${env.BUILD_ID}"
              }
           }
       }
      
      stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/constructionapp:latest/constructionapp:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', verifyDeployments: true])
            }
        }
      
      
      }
      
  }





def getDockerTag(){
def tag = sh script: 'git rev-parse HEAD', returnStdout: true
return tag
}
