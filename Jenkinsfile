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
      
      
      stage('Deloying to kubernetes'){
            
             steps{
      
     kubeconfig(caCertificate: '''-----BEGIN CERTIFICATE-----
MIIDKzCCAhOgAwIBAgIRAJKai8wfhf69+Sk04XGjUdswDQYJKoZIhvcNAQELBQAw
LzEtMCsGA1UEAxMkYzgyODQ3NTctN2JjYS00MDYwLTk1ZGItODMzOGM0OTVjN2Nk
MB4XDTIwMDgyMTExNDkxOVoXDTI1MDgyMDEyNDkxOVowLzEtMCsGA1UEAxMkYzgy
ODQ3NTctN2JjYS00MDYwLTk1ZGItODMzOGM0OTVjN2NkMIIBIjANBgkqhkiG9w0B
AQEFAAOCAQ8AMIIBCgKCAQEA1iOSaRaT2fcqSJCrU75CmEJ65R5k7/Ts6QWvw+8L
NLvmpWgNQ8a1y8mqD9h6nhI7dIeR/oyMKXE2XMcXwzGDqIOryixriFRHqslagdC3
xN/P8VTnDYE1bzO/dBlBTyQRaTmuYIV0sheJ/HR0yYea70M8fh6P1LfT8X6RxYdx
V8FNS2+1FaPsQ/CuARvSDBEVMsSr9k5x6Qi3B6L/Rr3dZQLE3G/o8Fv/Oc2h0uP0
INMSvPN2W4Xbf+HQq04iPWcpkFXXvRNip2t9jlUrDqXDcCOaO3FPoIYrET+QpJIy
tiY82ULQtXwbLNX7eK46qFVYutfRtK7EaAPR3X41Ee3/1QIDAQABo0IwQDAOBgNV
HQ8BAf8EBAMCAgQwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQU+Kk1UZnKamqa
ovfGKxPTEGqnarwwDQYJKoZIhvcNAQELBQADggEBAFtJBBdoXqxJ/68rFfbXfIJh
0azumtJJsiENE9t7oko9bCkCBHKMiAtgSeyv04DCKMhOmq8RIzhmnsQJqmGckvSL
IL2YxqVrNu0Hs3cYE7mxu1qcUrl0KffWL33f0zD9Zuk2LQtrqf/nrTMLxRHzGD7k
HvWFSN6iz+5TGVCzCf10T+AmIhKx3aPvYoDyRsf+5qvA0zW/ixU6OQuiPsJsu8Pm
/Sh/q91ks6OSZg50dW5wj8JXfc96V7RLEcXa1+M/qUfzktgi+XA2tLT4NIpyu5kV
qCR1iFIhwfhBDIo0p1gIA8mmQ68NrZti/osivkkqQrGW74vOGcl6Q8diVYulOuo=
-----END CERTIFICATE-----''', credentialsId: 'clusterauth', serverUrl: 'https://34.72.253.103') {
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
