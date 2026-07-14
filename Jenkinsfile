pipeline {
    agent any

    environment {
        IMAGE_NAME = "eekshithreddy/sample-app"
    }

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
                    echo "$DOCKER_PASS" | docker login \
                    -u "$DOCKER_USER" \
                    --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }
    }

    post {

        always {
            echo 'Pipeline Finished'
        }

        success {
            echo 'Docker Image Successfully Pushed'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}
