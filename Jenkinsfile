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
                sh 'mvn clean install'
            }
        }

       stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        
        stage('SonarQube') {
        steps {
             withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                withSonarQubeEnv('sonarqube-server') {
                    sh """
                         mvn sonar:sonar \
                        -Dsonar.projectKey=devopsproject \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                         -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t abirbenbrahem/devopsproject:latest .
                """
            }
        }

            stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub', 
                        usernameVariable: 'DOCKER_USERNAME', 
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh """
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push abirbenbrahem/devopsproject:latest
                    """
                }
            }
        }

    }

        post {
        success {
            mail to: 'benbrahemabir@gmail.com',
                 subject: 'Build Successful',
                 body: 'La build a réussii.'
        }
        failure {
            mail to: 'benbrahemabir@gmail.com',
                 subject: 'Build Failed',
                 body: 'La build a échoué.'
        }
    }

    
}

