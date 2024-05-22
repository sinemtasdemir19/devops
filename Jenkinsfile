pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DockerAuth')
        DOCKER_REGISTRY = 'sinemtasdemir'
        DOCKER_IMAGE = 'app'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Pull the project from GitHub') {
            steps {
                echo 'Getting the project from GitHub'
                git branch: 'main', url: 'https://github.com/sinemtasdemir19/devops.git'
            }
        }

        stage('Building the jar file') {
            steps {
                echo 'Start building the jar'
                bat './gradlew.bat clean bootJar' // Ensure gradlew.bat is executable
            }
        }

        stage('Create the Docker image of the application') {
            steps {
                echo 'Building the Docker image'
                powershell "docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${IMAGE_TAG} ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerAuth', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                    powershell 'docker login -u $env:DOCKERHUB_CREDENTIALS_USR --password $env:DOCKERHUB_CREDENTIALS_PSW'
                }
                echo 'Logged in'
            }
        }

        stage('Push the image to DockerHub') {
            steps {
                powershell "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${IMAGE_TAG}"
                echo 'The image is pushed'
            }
        }

        stage('Pull the image from DockerHub') {
            steps {
                echo 'Pulling the Docker image from DockerHub'
                powershell "docker pull ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${IMAGE_TAG}"
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    echo 'Deploying to Minikube'
                    powershell '''
                    kubectl apply -f src/deployment.yaml
                    kubectl apply -f src/service.yaml
                    '''
                }
            }
        }
    }
}
