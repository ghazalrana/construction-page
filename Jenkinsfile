pipeline {
    agent any
    environment {
        PROJECT_ID = ' wordpress-learning-277315'
        CLUSTER_NAME = 'cluster-1 '
        LOCATION = 'us-central1-c '
        CREDENTIALS_ID = 'GKE-credentials'
    }
   
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
          withKubeConfig([credentialsId: 'kubeconfig']) {
          sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f service.yaml'
        }
      }
  }
   }
}
    
       
        
  
