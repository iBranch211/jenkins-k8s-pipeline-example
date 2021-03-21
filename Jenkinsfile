pipeline {

  environment {
    imageName = "suayb/myweb"
    imageTag= "latest"
    dockerImage = ""
    registryCredential = 'dockerhub-cred'
  }

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build imageName + ":" + imageTag
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "", registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
