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
                 stage('Versioning') {
                    steps {
                        script {
                            env.COMMIT_HASH = sh(returnStdout: true, script: "git rev-parse HEAD | cut -c1-7").trim()
                            env.BUILD_DATE = sh(returnStdout: true, script: "date -u +'%d%m%y'").trim()
                            env.BUILD_VERSION = env.BUILD_DATE + env.COMMIT_HASH + env.BUILD_DATE
                            env.CONTAINER_TAG = env.BRANCH_NAME
                            if (env.BRANCH_NAME == 'master')
                            {
                                env.CONTAINER_TAG = 'release'
                            }
                            else if(env.BRANCH_NAME == 'dev')
                            {
                                env.CONTAINER_TAG = 'dev'
                            }
                        }
                        echo 'Build version: ' + env.BUILD_VERSION
                    }
                }
                stage('Build') {
                    parallel {
                        stage('Build backend image') {
                            steps {
                                dir('src/backend') {
                                    sh 'docker build -t  $CR_BACKEND:$CONTAINER_TAG-$BUILD_VERSION -t $CR_BACKEND:$CONTAINER_TAG-latest .'
                                }
                            }
                        }
                        // stage('Build frontend image') {
                        //     steps {
                        //         dir('src/frontend') {
                        //             sh 'docker build -t  $CR_FRONTEND:$CONTAINER_TAG-$BUILD_VERSION -t $CR_FRONTEND:$CONTAINER_TAG-latest .'
                        //         }
                        //     }
                        // }
                    }
                } 
                stage('Scan') {
                    steps {
                        sh 'trivy $CR_BACKEND:$CONTAINER_TAG-$BUILD_VERSION'
                    }
                }           
            }
        }        
    }
}