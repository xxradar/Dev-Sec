pipeline {
    agent any
    environment {
	}

    stages {
        stage('Build') {
            steps {
                script {
                    docker pull alpine
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
