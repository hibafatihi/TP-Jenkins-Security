pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'python3 -m pip install flask==0.12.2 requests==2.18.0 --break-system-packages'
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
                    sh '/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=. -Dsonar.host.url=http://172.17.0.3:9000'
                }
            }
        }
        stage('SCA Scan') {
            steps {
                sh '/opt/dependency-check/bin/dependency-check.sh --project "TP-Jenkins" --scan . --format HTML --failOnCVSS 7 --noupdate'
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities!'
        }
        success {
            echo 'Build successful!'
        }
    }
}
