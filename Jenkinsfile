pipeline {
	agent {
        docker {
            image 'node:20.11.0-alpine3.19' 
            args '-p 3000:3000' 
        }
    }

	stages {
		stage('Checkout') {
			steps {
				checkout scm
			}
		}
        stage('Client Tests') {
            steps {
                dir('client') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }
	}

}