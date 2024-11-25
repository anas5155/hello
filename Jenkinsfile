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
    }
}
