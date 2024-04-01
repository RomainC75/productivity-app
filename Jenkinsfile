pipeline {
    agent { label 'ubuntu18-agent' }

    tools { 
	nodejs "nodejs-20.12"
	git "git-agent"
    }
	
    environment {
	    MONGODB_URI = credentials('mongodb-uri')
	    TOKEN_KEY = credentials('token-key')
	    EMAIL = credentials('email')
	    PASSWORD = credentials('password')
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
	stage('Server Tests') {
	     steps {
                dir('server') {
                    sh 'npm install'
                    sh 'export MONGODB_URI=$MONGODB_URI'
			        sh 'export TOKEN_KEY=$TOKEN_KEY'
			        sh 'export EMAIL=$EMAIL'
			        sh 'export PASSWORD=$PASSWORD'
                    sh 'npm test'
                }
            }
	}
        stage('Build Image') {
            agent {
                dockerfile {
                    filename 'Dockerfile'
                    dir 'server'
                }
            }
            steps {
                echo '==============BUILD================================='
                echo 'Image built'
                echo '================END OF BUILD===================================='
            }
        }
        
    }
}
