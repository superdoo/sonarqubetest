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
                git 'https://github.com/superdoo/sonarqubetest.git'  // Replace with your GitHub repository URL
            }
        }
        stage('Check SonarQube Scanner') {
            steps {
                script {
                    // Check if sonar-scanner is available
                    sh 'which sonar-scanner'  // This will print the path to sonar-scanner if it's installed correctly
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQube') {  // Ensure 'MySonarQube' matches the name of your SonarQube server in Jenkins
                        sh '''
                          echo "Running SonarQube analysis..."
                          sonar-scanner \
                            -Dsonar.projectKey=sonarqubetest \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9090 \
                            -Dsonar.login=$SONARQUBE_TOKEN \
                            -X  # Enable debug logging
                        '''
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
