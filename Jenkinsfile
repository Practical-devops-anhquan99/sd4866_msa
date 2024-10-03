pipeline {
    agent {
        docker  { image 'node:alpine'}
    }
    stages {
        stage("Node version") {
            steps {
                sh 'node --version'
            }
        }
        stage('Build backend') {
            steps {
                sh 'ls && cat package-lock.json'
                // sh 'cd src/backend && rm -rf node_modules'
                // sh 'npm install'
            }
        }
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