pipeline {
  agent any
  stages {
    stage ('adding dependencies') {
      steps {
         sh 'sudo apt-get update'
         sh 'sudo apt-get install -y ca-certificates curl gnupg lsb-release'
         sh 'sudo mkdir -p /etc/apt/keyrings && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg'
         sh 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null'
         sh 'sudo apt-get update'
         sh 'sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y'
        
      
      }
    
    
    }
  	stage('Docker Build & Push') {
    	
      steps {
      	sh 'docker build -t public.ecr.aws/r9j4k4q3/lampserver:latest -f docker/.'
        sh 'aws ecr-public get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin public.ecr.aws/r9j4k4q3/lampserver'
        sh 'docker push public.ecr.aws/r9j4k4q3/lampserver:latest'
      }
    }
  }
}
