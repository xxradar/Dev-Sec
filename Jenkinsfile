pipeline {
    agent any
    environment {
	registry = "dockerfabric/helloworld" 
        registryCredential = 'jenkins' 
        dockerImage = '' 
	}
    stages {
        stage("Checkout sourcecode") {
            steps {
                checkout scm
            }
        }
        stage("FortiDevSec SAST Scanner-") {
            steps {
sh' docker pull registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
              }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("dockerfabric/helloworld:${env.BUILD_ID}")
                }
            }
        }
        stage("FortiCWP Image Scanner") {
            steps {
                script {
                     try {
                        fortiCWPScanner block: true, imageName: "dockerfabric/helloworld:${env.BUILD_ID}"
                        } catch (Exception e) {
    
                 echo "Request for Approval"  
                  }
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
        stage("Push image to DockerHub") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'jenkins') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }    
}
