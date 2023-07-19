pipeline {
  environment {
    dockerimagename = "nikhildocker28/frontend-k8s"
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
    stage('Pushing Image to DockerHub'){
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
          sh "kubectl apply -f front-deploy.yaml"
          sh "kubectl apply -f front-svc.yaml"
          sh "kubectl apply -f ingress.yaml"
        }
      }
    }
  }
}
