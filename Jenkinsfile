pipeline {
	agent any

	stages {
		stage('Checkout') {
			steps {
				checkout scm
			}
		}
        stage('Client Tests') {
            steps {
                dir('client') {
                    sh 'apt-get install nodejs npm'
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }
	}

}