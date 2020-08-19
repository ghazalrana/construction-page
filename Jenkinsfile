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
              withCredentials([usernameColonPassword(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u gzlkhan -p ${dockerHubPwd}"
                    sh "docker push gzlkhan/jenkinstesting:${DOCKER_TAG}"
              }
           }
       }
   }
}
    
       
        
   def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
