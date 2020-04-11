def label = 'builder'

podTemplate(label: label, containers: [
    containerTemplate(name: 'gradle', image: 'gradle:6.3-jdk8', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kaniko', image: 'gcr.io/kaniko-project/executor:v0.19.0', command: 'cat', ttyEnabled: true)
    ], volumes: [
    persistentVolumeClaim(mountPath: '/root/.m2/repository', claimName: 'maven-cache', readOnly: false),
    secretVolume(secretName: 'jenkins-docker-secret', mountPath: '/kaniko/.docker')
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
        stage('Build and push image') {
            container('kaniko') {
                sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=ryandjf/example-order-service'
            }
        }
    }
}
