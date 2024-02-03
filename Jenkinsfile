pipeline {
    agent any

    stages {
        stage('Pull from Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[credentialsId: 'Git-Credentials', url: 'https://github.com/Ramjogesh/website.git']]])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build docker image and save reference to dockerImage
                    dockerImage = docker.build("ramjogesh/mywebsite:${BUILD_NUMBER}", ".")
                    dockerImage.inside {
                        // Any additional steps inside the Docker container if needed
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Push the built image to Docker Hub
                    withDockerRegistry(credentialsId: 'Docker-Credential', url: 'https://index.docker.io/v1/') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}

