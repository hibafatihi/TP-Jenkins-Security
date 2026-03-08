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
        stage('SCA Scan') {
            steps {
                sh '/opt/dependency-check/bin/dependency-check.sh --project "TP-Jenkins" --scan . --format HTML --failOnCVSS 7 --noupdate'
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
    }
}
