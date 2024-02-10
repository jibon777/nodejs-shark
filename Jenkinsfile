 pipeline {
    agent any
    environment {
        PROJECT_ID = 'smartfren-labs'
        CLUSTER_NAME = 'jibon-gke'
        LOCATION = 'us-central1-b'
        CREDENTIALS_ID = 'jenkins-sa'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
            
        }
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
        stage('Deploy to GKE') {
            steps{
                step([
                $class: 'KubernetesEngineBuilder',
                projectId: env.PROJECT_ID,
                clusterName: env.CLUSTER_NAME,
                location: env.LOCATION,
                manifestPattern: 'manifest.yaml',
                credentialsId: env.CREDENTIALS_ID,
                verifyDeployments: true])
            }
        }  
}
}