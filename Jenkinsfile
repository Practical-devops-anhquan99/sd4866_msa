pipeline {
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
    }

    stages {
        stage('SonarQube scan') {
            agent 'Built-In Node'
            stages('SonarQube A\nalysis') {
                steps {
                    withSonarQubeEnv(installationName: 'SonarQube') { 
                        sh '${scannerHome}/bin/sonar-scanner --version'
                        dir('src/backend') {
                            sh '${scannerHome}/bin/sonar-scanner -Dsonar. -Dsonar.sources=. -Dsonar.projectKey=MSA'
                        }
                    }
                }
            }
            stages('Quality Gate') {
                steps {
                    timeout(time: 5, unit: 'MINUTES') { 
                        waitForQualityGate abortPipeline: true 
                    }
                }
            }
        }

        stage('Build and test') {
            agent {
                docker { image 'node:20-alpine' }
            }
            when {
                not {
                    branch 'PR-*'
                }
            }
            stages {
                stage('Node version') {
                    steps {
                        sh 'node --version'
                    }
                }
                stage('Build') {
                    parallel {
                        stage('Build backend') {
                            steps {
                                dir('src/backend') {
                                    sh 'rm -rf node_modules && npm ci' 
                                }
                            }
                        }
                        stage('Build frontend') {
                            steps {
                                dir('src/frontend') {
                                    sh 'rm -rf node_modules && npm ci' 
                                }
                            }
                        }
                    }
                }
                stage('Test') {
                    parallel {
                        stage('Test backend') {
                            steps {
                                dir('src/backend') {
                                    sh 'npm test'
                                }
                            }
                        }
                        stage('Test frontend') {
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
    }
}