pipeline {
    agent any
    environment {
        SONARQUBE_TOKEN = credentials('sonarqube-token')  // Use your stored SonarQube token
    }
    triggers {
        githubPush()  // This will trigger the pipeline on push to GitHub
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sonarqubetest.git'  // Replace with your GitHub repository URL
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQube') {
                        sh 'mvn clean install sonar:sonar -Dsonar.login=$SONARQUBE_TOKEN'
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'SonarQube analysis completed successfully!'
        }
        failure {
            echo 'SonarQube analysis failed.'
        }
    }
}
