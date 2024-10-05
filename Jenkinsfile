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
                            else if (env.BRANCH_NAME == 'sonar')
                            {
                                env.CONTAINER_TAG = 'sonar'
                            }
                        }
                        sh "echo 'Build version: $BUILD_VERSION'"
                        sh 'printenv'
                    }
                }
            }
        }        
    }
}