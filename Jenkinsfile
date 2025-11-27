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

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

         stage('Test') {
            steps {
                script {
                        sh 'mvn test'
                }
            }
            post {
                always {
                    script {
                            junit '*/target/surefire-reports/.xml'
                    }
                }
            }
        }

        stage('Package') {
            steps {
                script {
                        sh 'mvn package -DskipTests'
                    } 
                }
            }

       stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar',  fingerprint: true, allowEmptyArchive: true
            }
        }
    }
}
