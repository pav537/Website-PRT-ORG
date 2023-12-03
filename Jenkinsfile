#!groovy
pipeline{
	environment 
	{ 
         DOCKERHUB_CREDENTIALS= credentials('dockerhub')     
    	}

  agent {label "master"}
  
    stages
      {
        stage('Docker Build') 
        {
    	 steps 
              {
      	     	print("building docker image") 
             	sh "sudo chmod 777 /var/run/docker.sock"
             	sh "docker build -t pav537/prt-test ."
		}
	  }
	 stage('Login to Docker Hub') 
	{         
	 steps
		      	{                            
			sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                 
			echo 'Login Completed'                
		        }           
		} 
		
	 stage('Deploy our image') 
	   { 
            steps 
		{ 
		  sh "docker push pav537/prt-test:latest"			
            	} 
            }
         
     stage('create nodeport service')
        {
          steps {
      sh "kubectl create -f k8s.yml"
	    sh "kubectl get svc"
          }
        }
    }
}
