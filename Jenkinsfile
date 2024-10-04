pipeline {
    agent {
        docker { image 'node:20-alpine' }
    }
    environment {
        HOME = '.'
        npm_config_cache = 'npm-cache'
        SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {
        stage('Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${SCANNER_HOME}/bin/sonar-scanner"
                }
            }
        }
    }
}