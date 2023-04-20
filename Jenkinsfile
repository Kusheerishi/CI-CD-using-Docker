pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/Kusheerishi/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t docker-app:latest .' 
                sh 'docker push kusheerishi/docker-app:latest'
                //sh 'docker tag docker-app Kusheerishi/docker-app:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerHub", url: "https://hub.docker.com/repository/docker/kusheerishi/docker-app/general" ]) {
          sh  'docker push kusheerishi/docker-app:tagname'
        //  sh  'docker push kusheerishi/docker-app:tagname:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 kusheerishi/docker-app"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@65.2.63.13 run -d -p 8003:8080 kusheerishi/docker-app"
 
            }
        }
    }
	}
    
