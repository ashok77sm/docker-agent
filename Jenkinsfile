pipeline {
    agent {
        docker {
            label 'docker-agent'  // This label MUST match the label you configured in the Docker Cloud template
            args '-u 0:0' // Optional: Run as root inside the container if needed (be cautious)
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
