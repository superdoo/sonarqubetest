pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:/opt/sonar-scanner/bin:${env.PATH}"
        SONARQUBE_TOKEN = credentials('sonarqubetoken')  // ID of your secret token in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/superdoo/sonarqubetest.git', branch: 'main'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                script {
                    // Define the venv directory
                    def venvDir = 'venv'

            // Create the virtual environment (only if it doesn't exist)
                    sh """
                    python3 -m venv ${venvDir}
                    """
                    // Activate the virtual environment
                    // Linux-based activation command
                    sh """
                         . ${venvDir}/bin/activate
                    """
                    }
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
