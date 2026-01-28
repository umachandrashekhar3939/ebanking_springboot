pipeline {
    agent any
    tools {
        maven 'maven'
    }

    stages {
        stage('Git') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
