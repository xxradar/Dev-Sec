pipeline {
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerfabric-dockerhub')
	}

    stages {
        stage('Build') {
            steps {
                script {
                    myapp = docker.build("dockerfabric/hkcwp:${env.BUILD_ID}")
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
