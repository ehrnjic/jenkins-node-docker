pipeline {
    agent any

    stages {
        stage('Build image with node app') {
            steps {
                sh '''
                    docker build -t nodeapp:$BUILD_ID -f Dockerfile .
                    docker tag nodeapp:$BUILD_ID nodeapp:latest
                '''
            }
        }

        stage('Run test script inside container') {
            steps {
                sh '''
                    docker ps -f name=nodeapp-test -q | xargs --no-run-if-empty docker stop
                    docker run -d --rm --name nodeapp-test -p 9000:3000 nodeapp:latest
                    docker exec -i nodeapp-test npm test
                    docker stop nodeapp-test
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker ps -f name=nodeapp-prod -q | xargs --no-run-if-empty docker stop
                    docker run -d --rm --name nodeapp-prod -p 3000:3000 nodeapp:latest
                '''
            }
        }
    }
}