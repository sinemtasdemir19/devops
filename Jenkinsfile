pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'sinemtasdemir/devops'
        DOCKER_CREDENTIALS = 'Jenkins'
        DOCKER_IMAGE = ''
        KUBECONFIG_CREDENTIALS_ID = 'kubeconfig'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sinemtasdemir/project3--devops'
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
                    DOCKER_IMAGE = docker.build("${DOCKER_REGISTRY}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS) {
                        DOCKER_IMAGE.push()
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
                        kubectl --kubeconfig=kubeconfig apply -f deployment.yaml
                        kubectl --kubeconfig=kubeconfig apply -f service.yaml
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
