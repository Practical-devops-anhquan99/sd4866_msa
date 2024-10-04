pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {
        stage('Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${SCANNER_HOME}/bin/sonar-scanner"
                }
            }
        }
        stage('Build') {
            environment {
                HOME = '.'
                npm_config_cache = 'npm-cache'
            }
            agent {
                docker { 
                    image 'node:20-alpine' 
                    reuseNode true
                }
            }
            stages {
                stage('Build backend') {
                    steps {
                        dir('src/backend') {
                            sh 'rm -rf node_modules && npm ci' 
                        }
                    }
                }
            }
        }
    }
}