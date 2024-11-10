pipeline {
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
        scannerHome = tool 'SonarQube scanner'
        CR_BACKEND = 'anhquan99/msa-backend'
        CR_FRONTEND = 'anhquan99/msa-frontend' 
        DOCKER_USERNAME = credentials('docker-username')
        DOCKER_PASSWORD = credentials('docker-password')
        NEED_TRIVY = credentials('need-trivy')
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
                label 'Built-In'
            }
            when {
                not {
                    branch 'PR-*'
                }
            }
            stages {
                 stage('Versioning image') {
                    steps {
                        script {
                            env.COMMIT_HASH = sh(returnStdout: true, script: "git rev-parse HEAD | cut -c1-7").trim()
                            env.BUILD_DATE = sh(returnStdout: true, script: "date -u +'%d%m%y'").trim()
                            env.BUILD_VERSION = env.BUILD_DATE + env.COMMIT_HASH + env.BUILD_DATE
                            if (env.BRANCH_NAME == 'master')
                            {
                                env.CONTAINER_TAG = 'release'
                            }
                            else if(env.BRANCH_NAME == 'dev')
                            {
                                env.CONTAINER_TAG = 'dev'
                            }
                        }
                        echo 'Build version: $BUILD_VERSION'
                    }
                }
                stage('Build image') {
                    parallel {
                        stage('Build backend image') {
                            steps {
                                dir('src/backend') {
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
                    }
                }
                stage('Scan image vulnerabilities') {
                    parallel {
                        stage('Scan backend image') {
                            steps {
                                sh 'trivy image  --no-progress $NEED_TRIVY --severity HIGH,CRITICAL $CR_BACKEND:$CONTAINER_TAG-$BUILD_VERSION'
                            }
                        }
                        stage('Scan frontend image') {
                            steps {
                                sh 'trivy image  --no-progress $NEED_TRIVY --severity HIGH,CRITICAL $CR_FRONTEND:$CONTAINER_TAG-$BUILD_VERSION'
                            }
                        }
                    }
                } 
                stage('Log into container registry') {
                    steps {
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    }
                } 
                stage('Push image') {
                    parallel {
                        stage('Push backend image'){
                            steps{
                                sh 'docker push $CR_BACKEND:$CONTAINER_TAG-$BUILD_VERSION'
                                sh 'docker push $CR_BACKEND:$CONTAINER_TAG-latest'
                            }
                        }
                        stage('Push frontend image'){
                            steps{
                                sh 'docker push $CR_FRONTEND:$CONTAINER_TAG-$BUILD_VERSION'
                                sh 'docker push $CR_FRONTEND:$CONTAINER_TAG-latest'
                            }
                        }
                    }
                }            
            }
        }
        stage('Clean up') {
            agent {
                label 'Built-In'
            }
            when {
                not {
                    branch 'PR-*'
                }
            }
            steps {
                sh 'docker rmi -f $(docker images | grep $CR_BACKEND | tr -s " " | cut -d " " -f 3)'
                sh 'docker rmi -f $(docker images | grep $CR_FRONTEND | tr -s " " | cut -d " " -f 3)'
            }
        }     
    }
    post {
        always {
            cleanWs(deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true)
        }
    }
}