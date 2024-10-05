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
                // stage('Build') {
                //     parallel {
                //         stage('Build backend image') {
                //             steps {
                //                 dir('src/backend') {
                //                     sh 'docker build -t $CR_BACKEND:$CONTAINER_TAG-$BUILD_VERSION -t $CR_BACKEND:$CONTAINER_TAG-latest .'
                //                 }
                //             }
                //         }
                //         stage('Build frontend image') {
                //             steps {
                //                 dir('src/frontend') {
                //                     sh 'docker build -t $CR_FRONTEND:$CONTAINER_TAG-$BUILD_VERSION -t $CR_FRONTEND:$CONTAINER_TAG-latest .'
                //                 }
                //             }
                //         }
                //     }
                // } 
                // stage('Log into container registry') {
                //     steps {
                //         sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                //     }
                // } 
                // stage('Push image') {
                //     parallel {
                //         stage('Push backend image'){
                //             steps{
                //                 sh 'docker push $CR_BACKEND:$CONTAINER_TAG-$BUILD_VERSION'
                //                 sh 'docker push $CR_BACKEND:$CONTAINER_TAG-latest'
                //             }
                //         }
                //         stage('Push frontend image'){
                //             steps{
                //                 sh 'docker push $CR_FRONTEND:$CONTAINER_TAG-$BUILD_VERSION'
                //                 sh 'docker push $CR_FRONTEND:$CONTAINER_TAG-latest'
                //             }
                //         }
                //     }
                // }              
            }
        }        
    }
}