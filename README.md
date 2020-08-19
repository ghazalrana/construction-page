# construction-page
jenkins pipeline on GKE


tutorials

https://github.com/kemallaydin/Configuring-CI-CD-on-Kubernetes-with-Jenkins

https://github.com/ghazalrana/jenkins.git

https://github.com/ghazalrana/jenkins/tree/master/HA-Jenkins-k8s

https://www.youtube.com/watch?v=dUr1s9qTQlk

https://www.youtube.com/watch?v=mRRS3gcC800

https://www.youtube.com/watch?v=naUhXrV_rRA

https://medium.com/@prasenjitsarkar_79320/highly-available-jenkins-deployment-on-google-kubernetes-engine-c2d9eeaeb45a




buildnumber ===> github commit id (sample)


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
   }
}
    
       
        
   def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
