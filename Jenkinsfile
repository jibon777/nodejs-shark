pipeline {
    agent any
    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImageTag = "asia.gcr.io/smartfren-labs/nodejs-shark/nodejs-shark:latest" // or your desired tag
                    docker.build(dockerImageTag, "-f Dockerfile .")
                    docker.withRegistry('https://asia.gcr.io', 'jenkins-sa') {
                        docker.image(dockerImageTag).push()
                    }
                }
            }
        }
    }
}