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
                label 'Built-In Node'
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
        stage('Build and publish image'){
            agent {
                label 'Built-In Node'
            }
            stages {
                 stage('Versioning') {
                    steps {
                        script {
                            env.COMMIT_HASH = sh(returnStdout: true, script: "git rev-parse HEAD | cut -c1-7").trim()
                            env.BUILD_DATE = sh(returnStdout: true, script: "date -u +'%d%m%y'").trim()
                            env.BUILD_VERSION = env.BUILD_DATE + env.COMMIT_HASH + env.BUILD_DATE
                            if (env.BRANCH_NAME == 'master')
                            {
                                env.CONTAINER_TAG = 'release'
                            }
                            else (env.BRANCH_NAME == 'dev')
                            {
                                env.CONTAINER_TAG = 'dev'
                            }
                        }
                        echo 'Build version: $BUILD_VERSION'
                    }
                }
                stage('Build') {
                    parallel {
                        stage('Build backend image') {
                            steps {
                                dir('src/backend') {
                                    if()
                                    sh 'docker build -t  $CR_BACKEND:$CONTAINER_TAG-$BUILD_VERSION -t $CR_BACKEND:$CONTAINER_TAG-latest .'
                                }
                            }
                        }
                        stage('Build frontend image') {
                            steps {
                                dir('src/frontend') {
                                    sh 'docker build -t  $CR_FRONTEND:$CONTAINER_TAG-$BUILD_VERSION -t $CR_FRONTEND:$CONTAINER_TAG-latest .'
                                }
                            }
                        }
                        stage('Log into container registry') {
                            steps {
                                sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                            }
                        }
                        stage('Pus docker image'){
                            steps{
                                sh 'docker push $CR_FRONTEND:$CONTAINER_TAG-$BUILD_VERSION'
                                sh 'docker push $CR_FRONTEND:$CONTAINER_TAG-latest'
                            }
                        }
                    }
                }                
            }
        }        
    }
}