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
sh '''#!/bin/bash
docker pull registry.fortidevsec.forticloud.com/fdevsec_sast:latest
docker run -i --rm --mount type=bind,source="$(pwd)",target=/scan registry.fortidevsec.forticloud.com/fdevsec_sast:latest
'''
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
	     
	    stage('Deploy Docker Image Locally') {
                steps {
                 sh '''#!/bin/bash
	            env
		    docker run -d --name helloserver -p 5000:5000 "dockerfabric/helloworld:${BUILD_ID}"
		    docker ps -a
                 '''
    }
}
	     stage("FortiDevSec DAST Scanner-") {
            steps {
sh '''#!/bin/bash
docker pull registry.fortidevsec.forticloud.com/fdevsec_dast:latest
docker run -i --rm --mount type=bind,source="$(pwd)",target=/scan registry.fortidevsec.forticloud.com/fdevsec_dast:latest
docker rm helloserver
'''    
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
      stage('Deploy to Self-Managed Kubernetes') {
           steps {
            sh '''#!/bin/bash
	        export KUBECONFIG=/home/my_jenkins_home/.kube/config
                kubectl apply -f deployment.yaml
            '''
    }
}
    }    
}
