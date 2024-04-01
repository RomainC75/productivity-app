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
                    sh 'npm install'
                    sh 'npm test'
                }
            }
            steps {
                dir('server') {
                    sh 'npm install'
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
