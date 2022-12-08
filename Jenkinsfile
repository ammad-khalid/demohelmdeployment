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
    /*stage ('adding dependencies') {
      steps {
         sh 'apt-get update && apt-get install -y unzip'
         sh 'yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo'
         sh 'yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y'
         sh 'systemctl start docker'
         sh 'rm -rf awscliv2.zip awscliv2 aws'
         sh 'curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"'
         sh 'unzip awscliv2.zip'
         sh './aws/install --update'
         
       
      }
    }*/
    /*stage ('openssl') {
      steps {
      sh 'yum install make gcc perl pcre-devel zlib-devel wget -y'
      sh 'wget https://ftp.openssl.org/source/old/1.1.1/openssl-1.1.1.tar.gz' 
      sh 'tar xvf openssl-1.1.1.tar.gz'
      sh 'cd openssl-1.1.1/ && ls -lha'
      sh './config --prefix=/usr --openssldir=/etc/ssl --libdir=lib no-shared zlib-dynamic'
      sh 'make && make test && make install'
      sh 'export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/lib64'
      sh 'echo "export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/lib64" >> ~/.bashrc'
      sh 'openssl version'
      }
    }*/
    /*stage ('helm install') {
        steps {
        sh 'wget https://get.helm.sh/helm-v3.6.3-linux-amd64.tar.gz'
        sh 'tar -xvf helm-v3.6.3-linux-amd64.tar.gz'
        sh 'mv linux-amd64/helm /usr/local/bin/helm'
        sh 'helm version'  
        }
      
      }
    
    */
  	stage('Docker Build & Push') {
    	
      steps {
        sh 'pwd && ls -lha'
        sh 'docker build -t 779160054397.dkr.ecr.us-east-1.amazonaws.com/lampserver:${BUILD_ID} -f docker/Dockerfile .'
        sh 'aws get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 779160054397.dkr.ecr.us-east-1.amazonaws.com/lampserver'
        sh 'docker push 779160054397.dkr.ecr.us-east-1.amazonaws.com/lampserver:${BUILD_ID}'
      }
    }
    stage ('eks connection') {
      steps {
        sh 'aws configure set aws_access_key_id "${KEY_ID}"' 
        sh 'aws configure set aws_secret_access_key "${KEY_SECRET}"'
      sh 'aws configure set region "eu-central-1"'
      sh 'aws configure set output "json"'
      /*sh 'cat ~/.aws/credentials'*/
      sh 'aws eks --region eu-west-1 update-kubeconfig --name jenkins'
      sh 'helm upgrade --install lamp -n app1 kubernetes-lamp/.'  
      }
    
    }
  }

}
