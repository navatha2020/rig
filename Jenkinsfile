pipeline {

  environment {
    registry = "https://registry.hub.docker.com"
     registryCredential = 'dockerhub'
    
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
               git 'https://github.com/navatha2020/justmecode.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build("navatha2020/myweb"  + ":$BUILD_NUMBER")
	}
      }
    }     
	  stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "https://registry.hub.docker.com/",registryCredential ) {
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
