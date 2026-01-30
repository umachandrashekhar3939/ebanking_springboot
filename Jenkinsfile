pipeline {
    agent any

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
                sh '''
                    echo "Building Spring Boot application..."
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('Stop Old Application') {
            steps {
                sh '''
                    echo "Stopping old application..."
                    pkill -f $JAR_NAME || true
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    echo "Deploying application..."
                    mkdir -p $DEPLOY_PATH
                    cp target/*.jar $DEPLOY_PATH/$JAR_NAME
                '''
            }
        }

        stage('Start Application') {
            steps {
                sh '''
                    echo "Starting application..."
                    nohup java -jar $DEPLOY_PATH/$JAR_NAME > $DEPLOY_PATH/app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo "ebanking application deployed successfully"
        }
        failure {
            echo "Build or deployment failed"
        }
    }
}
