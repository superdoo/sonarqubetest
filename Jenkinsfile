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

        stage('Install Dependencies') {
            steps {
                script {
                    // Create virtual environment
                    sh 'python3 -m venv venv'
                    
                    // Ensure proper permissions for virtual environment (if sudo works)
                    sh 'sudo chown -R jenkins:jenkins venv'  // Only needed if you want to use sudo
                    
                    // Alternatively, set permissions without sudo (less secure)
                    // sh 'chmod -R 777 venv'  // Use this if you do not want to configure sudoers file
                    
                    // Activate the virtual environment and install dependencies
                    sh 'source venv/bin/activate && pip install --upgrade pip && pip install pytest pytest-cov'
                }
            }
        }

        stage('Run Tests and Generate Coverage') {
            steps {
                script {
                    // Run tests with coverage inside the virtual environment
                    sh 'source venv/bin/activate && pytest --cov=src.app --cov-report=xml'
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
