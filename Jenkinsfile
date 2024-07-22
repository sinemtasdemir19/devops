pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'sinem/webapp'
        DOCKER_CREDENTIALS = 'DockerAuth'
        DOCKER_IMAGE = ''
        KUBECONFIG_CREDENTIALS_ID = 'kubeconfig'  
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sinemtasdemir19/devops'
            }
        }

        stage('Build JAR') {
            steps {
                script {
                    bat './gradlew clean bootJar'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t sinem/webapp:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        bat "docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%"
                        bat "docker push sinem/webapp:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([string(credentialsId: KUBECONFIG_CREDENTIALS_ID, variable: 'KUBECONFIG_CONTENT_BASE64')]) {
                        writeFile file: 'kubeconfig.base64', text: KUBECONFIG_CONTENT_BASE64
                        bat '''
                        if exist kubeconfig del kubeconfig
                        certutil -decode kubeconfig.base64 kubeconfig
                        kubectl --kubeconfig=kubeconfig apply -f C:\\Users\\sinem\\.minikube\\deployment.yaml
                        kubectl --kubeconfig=kubeconfig apply -f C:\\Users\\sinem\\.minikube\\service.yaml
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            echo 'Pipeline finished.'
        }
    }
}
