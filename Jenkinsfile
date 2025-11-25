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
                sh '''mvn clean package -DskipTests'''
            }
        }

       stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
            }
        }
    }
}
