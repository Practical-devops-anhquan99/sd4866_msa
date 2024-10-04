pipeline {
    agent any
    environment {
        scannerHome = tool 'SonarQube scanner'
    }
    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(installationName: 'SonarQube') { 
                    sh '${scannerHome}/bin/sonar-scanner --version'
                    dir('src/backend') {
                        sh '${scannerHome}/bin/sonar-scanner -Dsonar. -Dsonar.sources=.'
                        // sh 'sonar-scanner -Dsonar. -Dsonar.sources=.' 
                    }
                }
            }
        }

        // stage('Quality Gate') {
        //     steps {
        //         timeout(time: 5, unit: 'MINUTES') { 
        //             waitForQualityGate abortPipeline: true 
        //         }
        //     }
        // }
        // stage('Build') {
        //     environment {
        //         HOME = '.'
        //         npm_config_cache = 'npm-cache'
        //     }
        //     agent {
        //         docker { 
        //             image 'node:20-alpine' 
        //         }
        //     }
        //     stages {
        //         stage('Build backend') {
        //             steps {
        //                 dir('src/backend') {
        //                     sh 'rm -rf node_modules && npm ci' 
        //                 }
        //             }
        //         }
        //     }
        // }
    }
}