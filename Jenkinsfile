pipeline {
    agent {
        docker { image 'python:3.11-alpine' }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install --upgrade pip'
                sh 'pip install -r requirements.txt'
            }
        }   
    }
}
