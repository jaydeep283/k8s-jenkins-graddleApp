pipeline {
    agent any
    environment {
        dockerimagename = "genesispatil/graddleapp"
        dockerImage = ""
    }
    stages {
        stage('Checkout code') {
          steps{
              git branch: 'main', url: 'https://github.com/jaydeep283/k8s-jenkins-graddleApp.git'          }
        }

        stage('Graddle Build') {
          steps{
              sh './gradlew build'
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
