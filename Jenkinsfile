pipeline {
    agent any
    environment {
        SONARQUBE_TOKEN = credentials('sonarqubetoken') // Assuming you store your SonarQube token in Jenkins' credentials
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/superdoo/sonarqubetest.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build steps go here, e.g., compile the code or install dependencies
                    echo 'Building the project...'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Example for running Python tests with pytest (adjust according to your project)
                    echo 'Running tests...'
                    sh 'pytest --maxfail=1 --disable-warnings -q'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Running the SonarQube analysis with token authentication
                    withSonarQubeEnv('MySonarQube') {
                        sh 'sonar-scanner -Dsonar.projectKey=sonarqubetest -Dsonar.sources=src -Dsonar.host.url=http://localhost:9090 -Dsonar.token=$SONARQUBE_TOKEN'
                    }
                }
            }
        }

        stage('Post Analysis') {
            steps {
                script {
                    // Optionally add logic to fetch and display results or handle post-analysis
                    echo 'SonarQube analysis completed!'
                }
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
