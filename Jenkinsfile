pipeline {
    agent any 

    stages {
        stage('build') {
            steps {
                sh './gradlew clean build'
            }
        }
    }
    post {
        always {
            publishHTML target: [
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'build/reports',
                reportFiles: '**/*',
                reportName: 'Build Reports'
            ]
        }
    }
}