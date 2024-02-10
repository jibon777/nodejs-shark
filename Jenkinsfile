pipeline {
    agent any
    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImageTag = "asia.gcr.io/smartfren-labs/gke-jenkins:latest" // or your desired tag
                    docker.build(dockerImageTag, "-f Dockerfile .")
                    docker.withRegistry('https://asia.gcr.io', 'gcrjenkins') {
                        docker.image(dockerImageTag).push()
                    }
                }
            }
        }
    }
}