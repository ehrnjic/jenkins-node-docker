node {
    
    checkout scm

    def customImage = docker.build("nodeapp:${env.BUILD_ID}")
    customImage.tag("latest")
    
    customImage.inside {
        sh 'npm test'
    }

    stage('docker stop container') {
        def currContainer = docker.container('nodewebapp')
        currContainer.stop()
    }
    
    customImage.run("--rm -p 3000:3000 --name nodewebapp")
}