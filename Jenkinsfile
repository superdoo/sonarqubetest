pipeline {
 agent any
    environment {
        SONARQUBE_TOKEN = credentials('sonarqubetest')  // Replace with your token name
    }
     stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/superdoo/sonarqubetest.git'  // Replace with your GitHub URL
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQube') {
                        sh ls -l
                        sh 'sonar-scanner -Dsonar.projectKey=sonarqubetest -Dsonar.sources=. -Dsonar.login=$SONARQUBE_TOKEN'
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
