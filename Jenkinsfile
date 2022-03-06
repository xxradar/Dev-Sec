pipeline {
    agent any
    environment {
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm   
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("dockerfabric/hello-world:${env.BUILD_ID}")
                }
            }
        }
             stage('Code approval request') {
     
           steps {
             script {
               def userInput = input(id: 'confirm', message: 'Do you Approve to use this code?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Approve Code to Proceed', name: 'approve'] ])
              }
            }
          }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        }
    }    
}
