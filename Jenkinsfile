pipeline {
    agent any
    environment {
        scannerHome = tool 'SonarQube scanner'
    }
    stages {
        stage('Versioning') {
            steps {
                script {
                    env.COMMIT_HASH = sh(returnStdout: true, script: "git rev-parse HEAD | cut -c1-7").trim()
                    env.BUILD_DATE = sh(returnStdout: true, script: "date -u +'%d%m%y'").trim()
                    env.BUILD_VERSION = '$env.BUILD_DATE$$env.COMMIT_HASH$env.BUILD_DATE'
                }
                sh 'echo "$BUILD_VERSION"'
            }
        }
    }
}