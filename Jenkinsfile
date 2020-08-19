pipeline {
    agent any
   
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t gzlkhan/constructionapp:${env.BUILD_NUMBER}"
            }
        }
         stage('Dockerhub Push'){
            
             steps{
              withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u gzlkhan -p ${dockerHubPwd}"
                    sh "docker push gzlkhan/constructionapp:${env.BUILD_NUMBER}"
              }
           }
       }
        stage('Docker Remove Image') {
      steps {
          sh "docker rmi gzlkhan/constructionapp:${env.BUILD_NUMBER}"
      }
    }
   }
}
    
       
        
  
