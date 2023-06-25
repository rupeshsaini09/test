pipeline {
  environment {
    dockerimagename = "rupeshsaini09/rupesh-webapp"
    dockerImage = ""
  }
  agent { node { label 'agent' } } 
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/rupeshsaini09/test.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
               registryCredential = 'docker-hub-creds'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying webapp to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploy.yml")
        }
      }
    }
  }
}
