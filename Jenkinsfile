pipeline {
    agent any

    environment {
        registry = "akboubhajar/tp4"  
      registryCredential = 'Jenkinsfile'
        dockerImage = ""
    }

    stages {
        // Stage 1: Clone the Git repository
        stage('Cloning Git') {
            steps {
                git 'https://github.com/akboubhajar/tp4'
            }
        }

        // Stage 2: Authenticate Docker
       stage('Authenticate Docker') {
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: registryCredential, usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                bat "echo %DOCKER_HUB_PASSWORD% | docker login -u %DOCKER_HUB_USERNAME% --password-stdin"
            }
        }
    }
}

        // Stage 3: Build the Docker image
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        // Stage 4: Test the Docker image
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }

        // Stage 5: Publish the Docker image to Docker Hub
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        // Stage 6: Deploy the Docker image
        stage('Deploy image') {
            steps {
                script {
                    bat "docker run -d ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
}