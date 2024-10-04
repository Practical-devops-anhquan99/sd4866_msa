pipeline {
    agent {
        docker { image 'node:20-alpine' }
    }
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
    }

    stages {
        stage('Checkout') { 
            steps {
                checkout scm 
            }
        }

        stage('For PR') {
            when {
                branch 'PR-*'
            }
            stages {
                stage('Hello PR') {
                    steps {
                        echo 'Hello PR'
                    }
                }
            }
        }

        stage('For branch') {
            when {
                not {
                    branch 'PR-*'
                }
            }
            stages {
                stage('Hello branch'){
                    steps {
                        echo 'Hello branch'
                    }
                }
                // stage('Node version') {
                //     steps {
                //         sh 'node --version'
                //     }
                // }
                // stage('Build') {
                //     parallel {
                //         stage('Build backend') {
                //             steps {
                //                 dir('src/backend') {
                //                     sh 'rm -rf node_modules && npm ci' 
                //                 }
                //             }
                //         }
                //         stage('Build frontend') {
                //             steps {
                //                 dir('src/frontend') {
                //                     sh 'rm -rf node_modules && npm ci' 
                //                 }
                //             }
                //         }
                //     }
                // }
                // stage('Test') {
                //     parallel {
                //         stage('Test backend') {
                //             steps {
                //                 dir('src/backend') {
                //                     sh 'npm test'
                //                 }
                //             }
                //         }
                //         stage('Test frontend') {
                //             steps {
                //                 dir('src/frontend') {
                //                     sh 'npm test'
                //                 }
                //             }
                //         }
                //     }
                // }
            }
        }
    }
}