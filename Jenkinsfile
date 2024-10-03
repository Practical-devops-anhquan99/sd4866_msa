pipeline {
    agent {
        docker  { image 'node:lts-buster'}
    }
    stages {
        stage("Node version") {
            steps {
                sh 'node --version'
            }
        }
        stage('Build') {
            parallel {
                stage('Build backend'){
                    steps {
                        sh 'cd src/backend && npm install'
                    }
                }
                stage('Build frontend'){
                    steps {
                        sh 'cd src/frontend && npm install'
                    }
                }
            }
        }
        stage('Test'){
            parallel {
                stage('Test backend'){
                    steps {
                        sh 'cd src/backend && npm test'
                    }
                }
                stage('Test frontend'){
                    steps {
                        sh 'cd src/frontend && npm test'
                    }
                }
            }
        }
 
    }
}