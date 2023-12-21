pipeline {
    agent any
    
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    // Checkout the code from the repository
                    git credentialsId: 'Git-Credentials' git branch: 'master', url: 'https://github.com/Ramjogesh/website.git'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {

                    sh 'docker build . -t mywebsite:latest'
                }
            }
        }
        
        stage('Build and Publish') {
            when {
                expression {
                    return env.BRANCH_NAME == 'master' || env.BRANCH_NAME == 'develop'
                }
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'master') {
                        // Build and publish website on port 82
                        sh 'docker run -v /var/www/html:/var/www/html -p 82:80 mywebsite:latest'
                    } else if (env.BRANCH_NAME == 'develop') {
                        // Only build the product without publishing
                        sh 'docker run -v /var/www/html:/var/www/html mywebsite:latest'
                    }
                }
            }
        }
    }
}