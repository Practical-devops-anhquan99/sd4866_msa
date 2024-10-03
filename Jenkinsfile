pipeline {
    agent {
        docker  { image 'node:20-alpine'}
    }
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
    }
    stages {
        stage('Node version') {
            steps {
                sh 'node --version'
            }
        }
        stage('Build')
        {
            parallel {
                stage('Build backend') {
                    steps {
                        dir('src/backend') {
                            sh 'rm -rf node_modules && npm install'
                        }
                    }
                }
                stage('Build frontend') {
                    steps {
                        dir('src/frontend') {
                            sh 'rm -rf node_modules && npm install'
                        }
                    }
                }
           }
        }
        stage('Test'){
            parallel {
                stage('Test backend'){
                    steps {
                        dir('src/backend') {
                            sh 'npm test'
                        }
                    }
                }
                stage('Test frontend'){
                    steps {
                        dir('src/frontend') {
                            sh 'npm test'
                        }
                    }
                }
            }
        }
 
    }
}