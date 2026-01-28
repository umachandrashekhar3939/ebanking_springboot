pipeline {
    agent any

    environment {
        APP_NAME = "ebanking-app"
        IMAGE_NAME = "ebanking-springboot"
        DOCKERHUB_USER = "your_dockerhub_username"
        DOCKER_IMAGE = "${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}"
    }

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/umachandrashekhar3939/ebanking_springboot.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS')]) {

                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push ${DOCKER_IMAGE}
                    """
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker rm -f ${APP_NAME} || true
                docker run -d --name ${APP_NAME} -p 8081:8080 ${DOCKER_IMAGE}
                """
            }
        }
    }

    post {
        success {
            echo "Build & Deployment Successful"
        }
        failure {
            echo "Pipeline Failed"
        }
    }
}

