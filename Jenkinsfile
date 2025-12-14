// flask uruchomiony z linii poleceń

pipeline {
    agent any
    
    
    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        
        stage('Run with venv') {
            options {
                timeout(time: 5, unit: 'MINUTES') 
                }

            
            steps {
                script {
                        sh '''
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install -r requirements.txt
                        python3 app.py
                        '''
                }
            }
        }
        
        
        
        stage('Verify Deployment') {
            steps {
                sh 'sleep 5'
                sh 'curl -f http://localhost:5000 || exit 1'
            }
        }
    }
    
    post {
        always {
            sh "docker image prune -f"
        }
    }
}

