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
        
        stage('Deploy to kubernetes') {
      steps {
          sh "chmod +x changeTag.sh"
          sh "./changeTag.sh ${BUILD_NUMBER}" 
          sshagent(['clusterssh']) {
    sh "scp -o StrictHostKeyChecking=no service.yaml node-app-pod.yml admin@35.223.70.19:/home/m_ahmad00861/"

          script{
                  try{
              sh "ssh admin@35.223.70.19 kubectl apply -f ." 
                  }catch(error){
                       sh "ssh admin@35.223.70.19 kubectl create -f ." 
                  }
              }
          }
           }
      }
  }
}
       
        
  
