pipeline {
    agent {
        docker {
            image 'docker:latest' // Use a Docker image with Docker CLI to build/run other Docker images
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Mount the host's Docker socket
        }
    }
    environment {
	    DOCKER_IMAGE = 'ashokm77/test-repo'
    }	
    stages {
            stage('checkout-stage') {
                steps {
                    git branch: 'master', credentialsId: 'ashoksm', url: 'https://github.com/ashok77sm/docker-sample-java-webapp.git'
                }
            }   
            stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:V${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Deploy Container on Agent with Volume') {
            steps {
                script {
                    sh "docker volume create docker-volume"

                    docker.image("${DOCKER_IMAGE}:V${env.BUILD_NUMBER}").run("-p 9000:8080 -v docker-volume:/app -d")
                }
            }
        }
    }
}
