pipeline {
  environment {
    dockerimagename = "nikhildocker28/k8s-db"
    dockerimage = ""
  }
  agent any
  
  stages {
    stage('Git Checkout') {
      steps {
        git 'https://github.com/nikgit7/node-js-project.git'
      }
    }
    
    stage('Build Image') {
      steps {
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Images to DockerHub'){
      environment {
        registryCredential = 'nikhildockerlogin'
      }
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential){
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploy to Minikube') {
      steps {
        script {
          sh "cd /home/ec2-user/.ssh"
          sh "ssh -i id_rsa ubuntu@35.172.134.200"
          sh "cd /home/ubuntu/app"
          sh "kubectl apply -f mysql-deploy.yaml"
          sh "kubectl apply -f mysql-svc.yaml"
          sh "kubectl apply -f ingress.yaml"
        }
      }
    }
  }
}
