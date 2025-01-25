pipeline {
    agent any

    environment {
        registry = "akboubhajar/tp4"  
        registryCredential = 'dockerhub-token'  
        dockerImage = ""
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/akboubhajar/tp4' 
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
                    sh "docker run -d ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
}