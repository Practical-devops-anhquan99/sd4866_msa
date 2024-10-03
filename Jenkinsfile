pipeline {
    agent {
        docker  { image 'node:alpine'}
    }
    stages {
        stage('Checkout specific directory'){
            dir('app') {
                checkout scm
            }
        }
        stage("Node version") {
            steps {
                sh 'node --version'
            }
        }
        stage('Remove package-lock.json'){
            steps {
                // sh 'rm package-lock.json'
                sh 'pwd'
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