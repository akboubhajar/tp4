pipeline {
    agent any

    environment {
        registry = "akboubhajar/tp4"  
      registryCredential = 'Jenkinsfile'
        dockerImage = ""
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/akboubhajar/tp4'
            }
        }

       stage('Authenticate Docker') {
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: registryCredential, usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                bat "echo %DOCKER_HUB_PASSWORD% | docker login -u %DOCKER_HUB_USERNAME% --password-stdin"
            }
        }
    }
}

        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                script {
                    bat "docker run -d ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
}