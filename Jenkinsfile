pipeline {
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
        scannerHome = tool 'SonarQube scanner'
        CR_BACKEND = 'anhquan99/msa-backend'
        CR_FRONTEND = 'anhquan99/msa-frontend' 
        DOCKER_USERNAME = credentials('docker-username')
        DOCKER_PASSWORD = credentials('docker-password')
    }
    agent any
    stages {
        stage('SonarQube scan') {
            agent {
                label 'Built-In'
            }
            stages {
                stage('SonarQube Analysis') {
                    steps {
                        withSonarQubeEnv(installationName: 'SonarQube') { 
                            sh '${scannerHome}/bin/sonar-scanner --version'
                            dir('src/backend') {
                                sh '${scannerHome}/bin/sonar-scanner -Dsonar. -Dsonar.sources=. -Dsonar.projectKey=MSA'
                            }
                        }
                    }
                }
                stage('Quality Gate')  {
                    steps {
                        timeout(time: 5, unit: 'MINUTES') { 
                            waitForQualityGate abortPipeline: true 
                        }
                    }
                }
                
            }
        }
    }
}
