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
                // Pull code from GitHub
                git 'https://github.com/sonarqubetest.git'  // Replace with your GitHub repository URL
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis with the correct project key
                    withSonarQubeEnv('MySonarQube') {  // Ensure 'MySonarQube' matches the name of your SonarQube server in Jenkins
                        // Run Maven with SonarQube analysis using the correct project key
                        sh 'mvn clean install sonar:sonar -Dsonar.projectKey=sonarqubetest -Dsonar.login=$SONARQUBE_TOKEN'
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
