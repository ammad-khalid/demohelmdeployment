pipeline {
  agent any
  environment {
    KEY_ID = credentials('KEY_ID')
    KEY_SECRET = credentials('KEY_SECRET')
    ACCOUNT_ID = credentials('ACCOUNT_ID')
    /*EMAIL = credentials('EMAIL')
    USERNAME = credentials('USERNAME')
    GH_TOKEN = credentials('GH_TOKEN')*/
  }
  stages {

    stage ('adding dependencies') {
      steps {
         sh 'yum update -y && yum install -y makecache unzip'
         sh 'rm -rf awscliv2.zip awscliv2 aws'
         sh 'curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"'
         sh 'unzip awscliv2.zip'
         sh './aws/install --update'
      }
    }
    
    stage ('helm install') {
        steps {
        sh 'wget https://get.helm.sh/helm-v3.6.3-linux-amd64.tar.gz'
        sh 'tar -xvf helm-v3.6.3-linux-amd64.tar.gz'
        sh 'mv linux-amd64/helm /usr/local/bin/helm'
        sh 'helm version'  
        }
      
      }
    
  	stage('Docker Build & Push') {
      
      steps {
        sh 'aws configure set aws_access_key_id "${KEY_ID}"' 
        sh 'aws configure set aws_secret_access_key "${KEY_SECRET}"'
        sh 'aws configure set region "eu-west-1"'
        sh 'aws configure set output "json"'
        sh 'aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin ${ACCOUNT_ID}.dkr.ecr.eu-west-1.amazonaws.com'

        sh 'pwd && ls -lha'
        sh 'docker build -t ${ACCOUNT_ID}.dkr.ecr.eu-west-1.amazonaws.com/lampserver:${BUILD_ID} -f docker/Dockerfile .'
        sh 'docker push ${ACCOUNT_ID}.dkr.ecr.eu-west-1.amazonaws.com/lampserver:${BUILD_ID}'
      }
    }
    stage ('change Tag in app manifest') {
      steps {
        sh 'git config --global user.email "${EMAIL}"'
        sh 'git config --global user.name "${USERNAME}"'
        sh 'git remote set-url origin https://${GH_TOKEN}@github.com/ammad-khalid/demohelmdeployment.git/'
        sh 'git config --replace-all remote.origin.fetch +refs/heads/*:refs/remotes/origin/*'
        
        
        sh 'cd ${WORKSPACE}'
        sh 'git fetch --tags'
        sh 'git checkout ${GIT_BRANCH}'
        sh 'sed -i "s/tag: .*/tag: ${BUILD_ID}/g" ${WORKSPACE}/kubernetes-lamp/values.yaml'
        sh 'git add ${WORKSPACE}/kubernetes-lamp/values.yaml'
        sh 'git commit --allow-empty -m "Version stamp ${BUILD_ID}"'
        sh 'git push origin HEAD:main --force'
      }
    }
    stage ('eks connection') {
      steps {
      sh 'cat ~/.aws/credentials'
      sh 'aws eks --region eu-west-1 update-kubeconfig --name jenkins'
      sh 'helm upgrade --install lamp -n app1 kubernetes-lamp/.'  
      }
    
    }
  }

}
