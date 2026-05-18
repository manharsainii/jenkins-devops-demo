pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "manhartrainings/jenkins-docker-demo"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/manharsainii/jenkins-devops-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% .'
            }
        }

        stage('Docker Login') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:%DOCKER_TAG%'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {

                bat """
                set KUBECONFIG=C:\Users\Dhruv\.kube\config
		kubectl set image deployment/flask-app-deployment flask-container=%DOCKER_IMAGE%:%DOCKER_TAG%
                """

            }
        }
    }
}
