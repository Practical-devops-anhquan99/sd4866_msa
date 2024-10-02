pipeline {
    agent any
    stages {
        stage('Echo') {
            steps {
                echo 'Hello'
            }
        }
        stage('for the PRs'){
            when {
                branch  'PR-*'
            }
            steps {
                echo 'This only runs for the PRs'
            }
        }
    }
}