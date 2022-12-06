pipeline {
  agent {
        docker { image 'ubuntu:18.04' }
    }
  stages {
  	stage('Docker Build & Push') {
    	
      steps {
      	sh 'docker build -t public.ecr.aws/r9j4k4q3/lampserver:latest -f docker/.'
        sh 'aws ecr-public get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin public.ecr.aws/r9j4k4q3/lampserver'
        sh 'docker push public.ecr.aws/r9j4k4q3/lampserver:latest'
      }
    }
  }
}
