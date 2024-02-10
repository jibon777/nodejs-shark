pipeline {
    agent any
    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImageTag = "asia.gcr.io/smartfren-labs/shark:latest" // or your desired tag
                    docker.build(dockerImageTag, "-f Dockerfile .")
                    docker.withRegistry('https://asia.gcr.io', 'gcr:gcrjekins') {
                        docker.image(dockerImageTag).push()
                    }
                }
            }
        }
    }
}