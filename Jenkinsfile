pipeline {
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
        scannerHome = tool 'SonarQube scanner'
        CR_BACKEND = credentials('cr-backend')
        CR_FRONTEND = credentials('cr-frontend')
        ECR_URI = credentials('ecr_uri')
        NEED_TRIVY = credentials('need-trivy')
        BACKEND_IMAGE = "${ECR_URI}/${CR_BACKEND}"
        FRONTEND_IMAGE = "${ECR_URI}/${CR_FRONTEND}"
        REGION = 'ap-southeast-1'
    }
    agent any
    stages {
        // stage('SonarQube scan') {
        //     agent {
        //         label 'Built-In'
        //     }
        //     stages {
        //         stage('SonarQube Analysis') {
        //             steps {
        //                 withSonarQubeEnv(installationName: 'SonarQube scanner') { 
        //                     sh '${scannerHome}/bin/sonar-scanner --version'
        //                     dir('src/backend') {
        //                         sh '${scannerHome}/bin/sonar-scanner -Dsonar. -Dsonar.sources=. -Dsonar.projectKey=MSA'
        //                     }
        //                 }
        //             }
        //         }
        //         stage('Quality Gate')  {
        //             steps {
        //                 timeout(time: 5, unit: 'MINUTES') { 
        //                     waitForQualityGate abortPipeline: true 
        //                 }
        //             }
        //         }
                
        //     }
        // }

        // stage('Build and test') {
        //     agent {
        //         docker { image 'node:20-alpine' }
        //     }
        //     when {
        //         not {
        //             branch 'PR-*'
        //         }
        //     }
        //     stages {
        //         stage('Node version') {
        //             steps {
        //                 sh 'node --version'
        //             }
        //         }
        //         stage('Build') {
        //             parallel {
        //                 stage('Build backend') {
        //                     steps {
        //                         dir('src/backend') {
        //                             sh 'rm -rf node_modules && npm ci' 
        //                         }
        //                     }
        //                 }
        //                 stage('Build frontend') {
        //                     steps {
        //                         dir('src/frontend') {
        //                             sh 'rm -rf node_modules && npm ci' 
        //                         }
        //                     }
        //                 }
        //             }
        //         }
        //         stage('Test') {
        //             parallel {
        //                 stage('Test backend') {
        //                     steps {
        //                         dir('src/backend') {
        //                             sh 'npm test'
        //                         }
        //                     }
        //                 }
        //                 stage('Test frontend') {
        //                     steps {
        //                         dir('src/frontend') {
        //                             sh 'npm test'
        //                         }
        //                     }
        //                 }
        //             }
        //         }
        //     }
        // }
        // stage('Build and publish image'){
        //     agent {
        //         label 'Built-In'
        //     }
        //     when {
        //         not {
        //             branch 'PR-*'
        //         }
        //     }
        //     stages {
        //          stage('Versioning image') {
        //             steps {
        //                 script {
        //                     env.COMMIT_HASH = sh(returnStdout: true, script: "git rev-parse HEAD | cut -c1-7").trim()
        //                     env.BUILD_DATE = sh(returnStdout: true, script: "date -u +'%d%m%y'").trim()
        //                     env.BUILD_VERSION = env.BUILD_DATE + env.COMMIT_HASH + env.BUILD_DATE
        //                     if (env.BRANCH_NAME == 'master')
        //                     {
        //                         env.CONTAINER_TAG = 'release'
        //                     }
        //                     else if(env.BRANCH_NAME == 'dev')
        //                     {
        //                         env.CONTAINER_TAG = 'dev'
        //                     }
        //                     else {
        //                         env.CONTAINER_TAG = 'test'
        //                     }
        //                 }
        //                 echo 'Build version: ${BUILD_VERSION}'
        //             }
        //         }
        //         stage('Build image') {
        //             parallel {
        //                 stage('Build backend image') {
        //                     steps {
        //                         dir('src/backend') {
        //                             sh 'docker build -t  ${BACKEND_IMAGE}:${CONTAINER_TAG}-${BUILD_VERSION} -t ${BACKEND_IMAGE}:${CONTAINER_TAG}-latest .'
        //                         }
        //                     }
        //                 }
        //                 stage('Build frontend image') {
        //                     steps {
        //                         dir('src/frontend') {
        //                             sh 'docker build -t  ${FRONTEND_IMAGE}:${CONTAINER_TAG}-${BUILD_VERSION} -t ${FRONTEND_IMAGE}:${CONTAINER_TAG}-latest .'
        //                         }
        //                     }
        //                 }
        //             }
        //         }
        //         stage('Scan image vulnerabilities') {
        //             steps {
        //                 sh 'trivy image --scanners vuln --no-progress ${NEED_TRIVY} --severity HIGH,CRITICAL ${BACKEND_IMAGE}:${CONTAINER_TAG}-${BUILD_VERSION}'
        //                 sh 'trivy image --scanners vuln --skip-db-update  --no-progress ${NEED_TRIVY} --severity HIGH,CRITICAL ${FRONTEND_IMAGE}:${CONTAINER_TAG}-${BUILD_VERSION}'
        //             }
        //         } 
        //         stage('Push image') {
        //             parallel {
        //                 stage('Push backend image'){
        //                     steps{
        //                         withAWS(region: REGION ,credentials:'aws-credential') {
        //                             sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URI}"
        //                             sh 'docker push $BACKEND_IMAGE:$CONTAINER_TAG-$BUILD_VERSION'
        //                             sh 'docker push $BACKEND_IMAGE:$CONTAINER_TAG-latest'
        //                         }
                                
        //                     }
        //                 }
        //                 stage('Push frontend image'){
        //                     steps{
        //                         withAWS(region: REGION ,credentials:'aws-credential') {
        //                             sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URI}"
        //                             sh 'docker push ${FRONTEND_IMAGE}:${CONTAINER_TAG}-${BUILD_VERSION}'
        //                             sh 'docker push ${FRONTEND_IMAGE}:${CONTAINER_TAG}-latest'
        //                         }
                                
        //                     }
        //                 }
        //             }
        //         }            
        //     }
        // }
        stage('Deploy k8s') {
            agent {
                label 'Built-In'
            }
            steps {
                withAWS(region: REGION ,credentials:'aws-credential') {
                    // sh "aws eks update-kubeconfig --name sd1121-devops-eks"
                    // sh "kubectl apply -f k8s"
                    sh "kubectl get nodes"
                }
            }
        }
        // stage('Clean up') {
        //     agent {
        //         label 'Built-In'
        //     }
        //     when {
        //         not {
        //             branch 'PR-*'
        //         }
        //     }
        //     steps {
        //         sh 'docker rmi -f $(docker images | grep $CR_BACKEND | tr -s " " | cut -d " " -f 3)'
        //         sh 'docker rmi -f $(docker images | grep $CR_FRONTEND | tr -s " " | cut -d " " -f 3)'
        //     }
        // }     
    }
    post {
        always {
            cleanWs(deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true)
        }
    }
}
