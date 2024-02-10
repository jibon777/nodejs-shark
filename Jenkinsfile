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
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("jibon/gke-node:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_id') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }    
        stage('Change Image GKE') {
                steps {
                    sh kubectl delete -f manifest.yaml
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