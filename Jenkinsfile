pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    environment {
        APP_NAME = "ebanking"
        DEPLOY_PATH = "/opt/ebanking"
        JAR_NAME = "ebanking.jar"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/umachandrashekhar3939/ebanking_springboot.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Stop Old Application') {
            steps {
                sh 'pkill -f $JAR_NAME || true'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    mkdir -p $DEPLOY_PATH
                    cp target/*.jar $DEPLOY_PATH/$JAR_NAME
                '''
            }
        }

        stage('Start Application') {
            steps {
                sh '''
                    nohup java -jar $DEPLOY_PATH/$JAR_NAME > $DEPLOY_PATH/app.log 2>&1 &
                '''
            }
        }
    }
}
