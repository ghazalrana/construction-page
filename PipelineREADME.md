
                                                        Jenkins pipeline on k8's
                                                        -------------------------
                                                        --------------------------
                                                        
                                                        
# Click on manageJenkins > Configue System

under "Jenkins Location" paste the jenkins url 

      http://IP-droplet:8080/
      
under "Global properties" click the checkbox "Environment variables" and enter the cerdentials of dockerhub password 

                                 Name: DOCKER_HUB

                                value:  <password-dockerhub>
                                
At the bottom of the page click on the cloud and enter configurations there

                              Name	:   kubernetes
 
                              Kubernetes URL   :    https://<master-node ip>       (you can get this ip by kubectl cluster-info or by endpoint in cluster details)
                              
                              Kubernetes Namespace : default
                              
                              Add credentials :   <create ur cluster credentials of type username and password from cluster details and make sure basic authentication is enable>
                              
                                                   if basic auth is not enable, click the cluster in console and edit it 
                                                   
                              	
                             	Jenkins URL  :   http://IP-droplet:8080/
                              


# Click on Manage Jenkins > Manage Plugins 

  install the following plugins 
  
                                         kubernetes
                                         
                                         docker
                                         
                                         git
                                         
                                         ssh Agent
                                         
 
 # Click New Item > write the name of ur pipeline > freestyle project
 
   click the checkbox "GitHub project"
              
              Project url:  <github-repo-url>
              
   under "Source Code Management" select "git" and add repository url
   
   
   Under "Build" select "Execute shell" and paste it 
   
   
                         IMAGE_NAME="gzlkhan/constructionapp:${BUILD_NUMBER}"
                         
                         docker build . -t $IMAGE_NAME
                         
                         docker login -u gzlkhan -p ${DOCKER_HUB}
                         
                         docker push $IMAGE_NAME
                         
                         
   Again select "Execute shell" and paste :
    
                          IMAGE_NAME="gzlkhan/constructionapp:${BUILD_NUMBER}"
                                  
                          kubectl set image deployment/myweb myweb=$IMAGE_NAME
                          
                          
   #Now ur job is set but u need to install docker ,kubectl and gcloud (for GKE) in the droplet
    
    for GKE => open terminal 
    
                             $ gcloud auth login
                             
                             and enter the connection command from console to the terminal to get the ~/.kube/config
                             
                             $ ~/.kube$  cp ./config  /home/jenkins/.kube
                             
                             
                             $ ~/.kube$  cp ./config  /var/lib/jenkins/.kube
                             
   and restart jenkins
    
                            $ sudo /etc/init.d/jenkins restart
                            
                            
                            
  tutorial:  
  
                                https://www.youtube.com/watch?v=288rTpd1SDE
                             
                             
                             
                         
                         
    
   
   
