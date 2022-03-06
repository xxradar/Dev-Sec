pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
            
            stage("FortiDevSec SAST Scanner-") {
            steps {
sh 'docker pull registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
   sh 'docker run --rm --mount type=bind,source=/var/lib/jenkins/workspace/FortiCWP_FortiDevSec_Demo,target=/scan registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
              }
        }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
