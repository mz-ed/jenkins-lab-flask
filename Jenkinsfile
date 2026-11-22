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
                sh "docker build -t ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ."
            }
        }

        stage('Test') {
            steps {
                sh "docker run --rm ${env.IMAGE_NAME}:${env.BUILD_NUMBER} python -m pytest -v tests"
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker rm -f ${env.CONTAINER_NAME} || true
                docker run -d --name ${env.CONTAINER_NAME} -p ${env.APP_PORT}:8000 -e APP_VERSION=${env.BUILD_NUMBER} ${env.IMAGE_NAME}:${env.BUILD_NUMBER}
                """
            }
        }

        stage('Verify') {
            steps {
                sh "curl --fail --retry 10 --retry-delay 2 --retry-connrefused http://localhost:${env.APP_PORT}/health"
                sh "curl --fail http://localhost:${env.APP_PORT}/"
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
