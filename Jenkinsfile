pipeline {
    agent {
        docker  { image 'node:alpine'}
    }
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
    }
    stages {
        stage("Node version") {
            steps {
                sh 'node --version'
            }
        }
        stage('Location'){
            steps {
                dir('src/backend') {
                    sh "npm install"
                }
                
            }
        }
        // stage('Build backend') {
        //     steps {
        //         sh 'cd src/backend && rm -rf node_modules'
        //         sh 'npm install'
        //     }
        // }
        // stage('Build frontend') {
        //     steps {
        //         sh 'cd ../frontend && rm -rf node_modules'
        //         sh 'npm install'
        //     }
        // }
        // stage('Test'){
        //     parallel {
        //         stage('Test backend'){
        //             steps {
        //                 sh 'cd src/backend && npm test'
        //             }
        //         }
        //         stage('Test frontend'){
        //             steps {
        //                 sh 'cd src/frontend && npm test'
        //             }
        //         }
        //     }
        // }
 
    }
}