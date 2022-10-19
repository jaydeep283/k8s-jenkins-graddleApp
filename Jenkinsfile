pipeline {

  environment {
    dockerimagename = "genesispatil/graddleapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage("Git Clone"){

        git 'https://github.com/jaydeep283/k8s-jenkins-graddleApp.git'
    }
    
    stage('Gradle Build') {

       sh './gradlew build'

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
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "k8s-spring-boot-deployment.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}