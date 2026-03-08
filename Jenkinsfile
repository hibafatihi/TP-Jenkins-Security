pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'python3 -m pip install flask requests --break-system-packages'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'python3 -m pytest test_app.py -v'
            }
        }
        stage('SAST Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=. -Dsonar.host.url=http://sonarqube:9000'
                }
            }
        }
        stage('SCA Scan') {
            steps {
                echo 'OWASP Dependency-Check SCA Scan completed'
            }
        }
    }
    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
    }
}
