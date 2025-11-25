pipeline {
    agent any 
    stages {
         stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/Abirbenbrahem-Git/jenkins.git']]
                )
            }
        }

        stage('Build & Install') {
            steps {
                sh 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test -Dmaven.test.failure.ignore=true'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }  

    post {
        success { echo "succeeded" }
        failure { echo "failed" }
    }
}
