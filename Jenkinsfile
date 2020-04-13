def label = "builder-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
    containerTemplate(
        name: 'gradle',
        image: 'gradle:6.3-jdk8',
        command: 'cat',
        ttyEnabled: true),
    containerTemplate(
        name: 'kaniko',
        image: 'gcr.io/kaniko-project/executor:debug',
        alwaysPullImage:true,
        command:'/busybox/sh -c',
        args:'/busybox/cat',
        ttyEnabled: true)
    ], volumes: [
        persistentVolumeClaim(mountPath: '/root/.m2/repository', claimName: 'maven-cache', readOnly: false),
        secretVolume(secretName: 'docker-config-secret', mountPath: '/kaniko/.docker')
    ], envVars: [
        envVar(key: 'SPRING_PROFILES_ACTIVE', value: 'jenkins')
    ]) {
    
    node(label) {
        checkout scm
        stage('Build') {
            container('gradle') {
                sh 'gradle clean build'
                publishHTML(target : [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/reports',
                    reportFiles: '**/*',
                    reportName: 'Build Reports'])
            }
        }
        stage('Build with Kaniko') {
            container('kaniko') {
                sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --destination=ryandjf/example-order-service:0.2.0'
            }
        }
    }
}
