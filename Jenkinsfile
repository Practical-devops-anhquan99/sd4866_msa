pipeline {
    agent {
        docker  { image 'node:alpine'}
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
        stage('Location'){
            steps {
                dir('src/backend') {
                    sh 'npm install'
                }
                
            }
        }
        stage('Build')
        {
            parallel {
                stage('Build backend') {
                    steps {
                        dir('src/backend') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Build frontend') {
                    steps {
                        dir('src/frontend') {
                            sh 'npm install'
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
                            sh 'ls node_modules/@babel'
                            sh 'npm test'
                        }
                    }
                }
            }
        }
 
    }
}