pipeline {
    agent any

    environment {
        registry = "666808198418.dkr.ecr.us-east-1.amazonaws.com/nextcloupapp"
    }

    tools {
                maven 'maven3.9.2'
            }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/Orgliontechtechfemi/nextcloud-application-docker-pipeline.git']]])
            }
        }
        
        stage("Build Image") {
            
            steps {
                script {
                    dockerImage = docker.build registry
                    dockerImage.tag(env.BUILD_NUMBER)
                }
            }
        }
        
        stage("Push to ECR") {
            steps {
                script {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 666808198418.dkr.ecr.us-east-1.amazonaws.com'
                    sh "docker push ${registry}:${env.BUILD_NUMBER}"
                }
            }
        }    
    }
}
