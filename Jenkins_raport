pipeline {
    agent any

    environment {
        PYTHON = 'python'
        VENV_DIR = '.venv'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python env') {
            steps {
                sh """
                ${PYTHON} -m venv ${VENV_DIR}
                . ${VENV_DIR}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run pytest') {
            steps {
                sh """
                . ${VENV_DIR}/bin/activate
                pytest -v --junitxml=reports/junit-report.xml
                """
            }
        }
    }

    post {
        always {
            // Wciągnięcie wyników testów do zakładki "Test Result"
            // junit 'reports/junit-report.xml'
            // blokada, bo wymaga odpowiedniej konfiguracji GitHub
            // https://stackoverflow.com/questions/67162746/how-to-get-rid-of-noisy-warning-no-suitable-checks-publisher-found
        }
        success {
            echo 'Tests passed 🎉'
        }
        failure {
            echo 'Tests failed ❌'
        }
    }
}