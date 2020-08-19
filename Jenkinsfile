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
        
        stage('Apply Kubernetes Files') {
      steps {
          sh "chmod +x changeTag.sh"
          sh "./changeTag.sh ${BUILD_NUMBER}" 
          withCredentials([usernamePassword(credentialsId: 'GKE-credentials', passwordVariable: 'GKEpwd', usernameVariable: 'GKEuser')]) {
              script{
                  try{
              sh "kubectl create -f node-app-pod.yml"
                  }catch(error){
                      sh " kubectl apply -f node-app-pod.yml
                  }
              }
       }
      }
  }
   }
}
    
       
        
  
