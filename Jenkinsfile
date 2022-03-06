pipeline {
    agent any
    stages {
        stage("Checkout sourcecode") {
            steps {
                checkout scm
                
            }
        }
        stage("FortiDevSec SAST Scanner-") {
            steps {
sh 'docker pull registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
   sh 'docker run --rm --mount type=bind,source=/var/lib/jenkins/workspace/FortiCWP_FortiDevSec_Demo,target=/scan registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
              }
        }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
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
}
