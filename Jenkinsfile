node {
    
    checkout scm

    def customImage = docker.build("nodeapp:${env.BUILD_ID}")
    customImage.tag("latest")
    
    customImage.inside {
        sh 'npm test'
    }
     
    customImage.run("--rm -p 3000:3000 --name nodewebapp")
}