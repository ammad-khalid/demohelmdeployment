pipeline {
  agent none
  stages {
    stage ('adding dependencies') {
      steps {
         sh 'yum update -y && yum install -y yum-utils makecache unzip'
         /*sh 'yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo'
         sh 'yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y'
         sh 'systemctl start docker'*/
         sh 'rm -rf *'
         sh 'curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"'
         sh 'unzip awscliv2.zip'
         sh './aws/install --update'
       
      }
    
    
    }
  	stage('Docker Build & Push') {
    	
      steps {
      	sh 'docker build -t public.ecr.aws/r9j4k4q3/lampserver:latest -f docker/Dockerfile .'
        sh 'aws ecr-public get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin public.ecr.aws/r9j4k4q3/lampserver'
        sh 'docker push public.ecr.aws/r9j4k4q3/lampserver:latest'
      }
    }
  }
}
