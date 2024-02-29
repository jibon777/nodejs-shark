 node {
    agent any
    environment {
        PROJECT_ID = 'smartfren-labs'
        CLUSTER_NAME = 'jibon-gke'
        LOCATION = 'us-central1-b'
        CREDENTIALS_ID = 'jenkins-sa'
    }
    stage('SCM') {
    checkout scm
    }
            stage('SonarQube Analysis') {
                def scannerHome = tool 'SonarScanner';
                withSonarQubeEnv() {
                sh "${scannerHome}/bin/sonar-scanner"
    }
    }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImageTag = "gcr.io/smartfren-labs/nodejs-shark:latest" // or your desired tag
                    docker.build(dockerImageTag, "-f Dockerfile .")
                    docker.withRegistry('https://gcr.io', 'gcr:gcrjekins') {
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
                manifestPattern: 'manifest.yaml',
                credentialsId: env.CREDENTIALS_ID,
                verifyDeployments: true])
            }
        }  
}