def label = 'builder'

podTemplate(label: label, containers: [
    containerTemplate(name: 'gradle', image: 'gradle:6.3-jdk8', command: 'cat', ttyEnabled: true)
    ], volumes: [
    persistentVolumeClaim(mountPath: '/root/.m2/repository', claimName: 'maven-cache', readOnly: false)
    ]) {
    
    node(label) {
        checkout scm
        stage('Build') {
            container('gradle') {
                sh 'gradle build'
                publishHTML(target : [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/reports',
                    reportFiles: '**/*',
                    reportName: 'Build Reports'])
            }
        }
    }
}
