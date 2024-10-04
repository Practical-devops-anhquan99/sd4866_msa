pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(installationName: 'YourSonarQubeInstallation') { 
                    dir('src/backend') {
                        sh 'sonar-scanner -Dsonar. -Dsonar.sources=.' 
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') { 
                    waitForQualityGate abortPipeline: true 
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