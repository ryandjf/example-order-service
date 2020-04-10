pipeline {
    agent {
        docker {
            image 'gradle:6.3-jdk8'
            args '-v $HOME/.gradle:/root/.gradle -v $HOME/.m2:/root/.m2'
        }
    } 

    stages {
        stage('build') {
            steps {
                sh 'gradle clean build'
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
