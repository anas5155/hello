pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/anas5155/hello.git'
            }
        }
        stage('Run Python Script') {
            steps {
            
                sh 'hello.py'
            }
        }
    }
    post {
        success {
            echo 'Python script executed successfully.'
        }
        failure {
            echo 'Python script execution failed.'
        }
	stage('Build Docker Image') {
            steps {
                script {
                    try {
                        docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                    } catch (Exception e) {
                        echo "Failed to build docker image: ${e.message}"
                        error "Failed to build docker image"
                    }
                }
            }
        }

	 stage('Push to DockerHub') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME',  passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh """
                                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                                docker push ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}
                            """
                        }
                    } catch (Exception e) {
                        echo "Failed to push Docker Image to registry: ${e.message}"
                        error "Failed to push Docker Image"
                    }
                }
            }
	}
    }
}
