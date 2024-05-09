 pipeline {
    agent any
    environment {
        PROJECT_ID = 'test-oracle-msig'
        CLUSTER_NAME = 'cluster-test-performance'
        LOCATION = 'asia-southeast2'
        CREDENTIALS_ID = 'jnks-msig'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
            
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImageTag = "gcr.io/test-oracle-msig/shark:latest" // or your desired tag
                    docker.build(dockerImageTag, "-f Dockerfile .")
                    docker.withRegistry('https://gcr.io', 'gcr:jnks-msig') {
                        docker.image(dockerImageTag).push()
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
                manifestPattern: 'deployment.yaml',
                credentialsId: env.CREDENTIALS_ID,
                verifyDeployments: true])
            }
        }

}
}