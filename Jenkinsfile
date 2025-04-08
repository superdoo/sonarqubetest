pipeline {
    agent any

    environment {
        SONARQUBE_TOKEN = credentials('sonarqubetest')  // ID of your secret token in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/superdoo/sonarqubetest.git', branch: 'main'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQube') {
                        sh """
                    export PATH=\$PATH:/opt/sonar-scanner/bin
                    sonar-scanner \
                      -Dsonar.projectKey=sonarqubetest \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=http://localhost:9090 \
                      -Dsonar.login=$SONARQUBE_TOKEN
                """
           }
        }
    }

    post {
        success {
            echo '✅ SonarQube analysis completed successfully!'
        }
        failure {
            echo '❌ SonarQube analysis failed.'
        }
    }
}
