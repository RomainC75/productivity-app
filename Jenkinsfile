pipeline {
    // agent { label 'ubuntu18-agent' }
    agent any

    tools { 
	nodejs "nodejs-20.12"
	git "git-default"
    }
	
    environment {
	    MONGODB_URI = credentials('mongodb-uri')
	    TOKEN_KEY = credentials('token-key')
	    EMAIL = credentials('email')
	    PASSWORD = credentials('password')
	    DOCKER_REGISTRY = 'factoregistry.azurecr.io'
        SERVER_IMAGE_NAME = 'myserverbuiltimage'
        CLIENT_IMAGE_NAME = 'myclientbuiltimage'
        IMAGE_TAG = 'latest'  
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
        stage('BUILD Docker Image') {
            steps {
                script {
                    sh 'echo ---------------------------'
                    sh 'echo ---------------------------'
                    sh 'ls'
                    sh 'echo $USER'
                    sh 'cd server'
                    sh 'ls -l'
                    docker.build("${env.DOCKER_REGISTRY}/${env.SERVER_IMAGE_NAME}:${env.IMAGE_TAG}")
                    sh 'cd ../client'
                    docker.build("${env.DOCKER_REGISTRY}/${env.CLIENT_IMAGE_NAME}:${env.IMAGE_TAG}", 'client')
                }
            }
        }
        stage('Build Images') {
            steps {
                sh 'docker build -t romainc75/productivity-app:client-latest client'
                sh 'docker build -t romainc75/productivity-app:server-latest server'
            }
        }
        
        stage('Push Images to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push romainc75/productivity-app:client-latest'
                    sh 'docker push romainc75/productivity-app:server-latest'
                }
            }
        }
        
    }
}
