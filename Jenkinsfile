pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DockerAuth')
    }

    stages {
        stage('Pull the project from GitHub') {
            steps {
                echo 'Getting the project from GitHub'
                git 'https://github.com/sinemtasdemir19/devops.git'
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
                bat 'docker build -t sinemtasdemir/app .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                bat 'echo %DOCKERHUB_CREDENTIALS_PSW% | docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin'
                echo 'Logged in'
            }
        }

        stage('Push the image to DockerHub') {
            steps {
                bat 'docker push sinemtasdemir/app'
                echo 'The image is pushed'
            }
        }
    }
}
