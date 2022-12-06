pipeline {
  agent any
  stages {
    /*stage('Checkout') {
        steps {
            git branch: 'main',
                credentialsId: '$demo',
                url: 'https://github.com/ammad-khalid/demohelmdeployment.git/'

            sh "ls -lat"
        }
    }*/
    stage ('adding dependencies') {
      steps {
         sh 'yum update -y && yum install -y yum-utils makecache unzip'
         /*sh 'yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo'
         sh 'yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y'
         sh 'systemctl start docker'*/
         sh 'rm -rf awscliv2.zip awscliv2 aws'
         sh 'curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"'
         sh 'unzip awscliv2.zip'
         sh './aws/install --update'
         
       
      }
    }
    stage ('helm install') {
        steps {
        sh 'curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3'
        sh 'chmod 700 get_helm.sh'
        sh './get_helm.sh'
        sh 'helm --version'  
        }
      
      }
    
    }
  	/*stage('Docker Build & Push') {
    	
      steps {
        sh 'pwd && ls -lha'
      	sh 'docker build -t public.ecr.aws/r9j4k4q3/lampserver:latest -f docker/Dockerfile .'
        sh 'aws ecr-public get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin public.ecr.aws/r9j4k4q3/lampserver'
        sh 'docker push public.ecr.aws/r9j4k4q3/lampserver:latest'
      }
    }*/
    stage ('eks connection') {
      steps {
      sh 'aws configure set aws_access_key_id "AKIA3K2MO5J64WESUUG5"'
      }
    
    }
  }
}
