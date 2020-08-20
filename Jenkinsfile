pipeline {

  environment {
    registry = "gzlkhan/constructionapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/ghazalrana/construction-page.git'
      }
    }

    stage('Build Docker Image'){
steps{
sh "docker build . -t gzlkhan/constructionapp:${env.BUILD_NUMBER}"
}
}

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
