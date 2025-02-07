pipeline {
    agent {
        docker { image 'python:3.11-alpine' }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install --upgrade pip --no-cache-dir'
                sh 'pip install --no-cache-dir --user -r requirements.txt'
            }
        }   
    }
}
