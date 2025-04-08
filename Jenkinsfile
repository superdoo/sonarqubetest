pipeline {
    agent any
    environment {
        SONARQUBE_TOKEN = credentials('sonarqubetoken')
    }
    stages {
        // Already checked out automatically by Jenkins
        // So skip this:
        // stage('Checkout') { steps { git 'https://github.com/superdoo/sonarqubetest.git' } }

        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQube') {
                        sh 'sonar-scanner -Dsonar.projectKey=sonarqubetest -Dsonar.sources=src -Dsonar.host.url=http://localhost:9090 -Dsonar.token=$SONARQUBE_TOKEN'
                    }
                }
            }
        }

        stage('Post Analysis') {
            steps {
                echo 'SonarQube analysis completed!'
            }
        }
    }
    post {
        success {
            echo 'Build and analysis completed successfully!'
        }
        failure {
            echo 'There was an error in the build or analysis.'
        }
    }
}
