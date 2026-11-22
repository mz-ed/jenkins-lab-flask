pipeline {
    agent any
    environment {
        IMAGE_NAME = 'jenkins-flask-lab'
        CONTAINER_NAME = 'jenkins-flask-app'
        APP_PORT = '8081'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is available in the workspace.'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t IMAGENAME:{BUILD_NUMBER} .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm IMAGENAME:{BUILD_NUMBER} pytest -v'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                docker rm -f ${CONTAINER_NAME} || true
                docker run -d --name ${CONTAINER_NAME} -p APPPORT: 8000 − eAPPVERSION =
                {BUILD_NUMBER} IMAGENAME:{BUILD_NUMBER}
                '''
            }
        }
        stage('Verify') {
            steps {
                sh 'curl --fail --retry 10 --retry-delay 2 http://localhost:${APP_PORT}/health'
                sh 'curl --fail http://localhost:${APP_PORT}/'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the stage that reported an error.'
        }
    }
}
