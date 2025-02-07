pipeline {
    agent {
        docker { 
            image 'python:3.11-alpine' 
            args '--user root'
        }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'apk add --no-cache git'
                sh 'pip install --user --upgrade pip --no-cache-dir'
                sh 'pip install --no-cache-dir --user -r requirements.txt'
            }
        }   
        stage('Run Code Formatters') {
            steps {
                sh 'python -m black .'
                sh 'python -m isort .'
                sh 'python -m autopep8 --in-place --recursive .'
            }
        }
        
        stage('Check for Changes') {
            steps {
                script {
                    def changes = sh(script: 'git status', returnStdout: true).trim()
                    if (changes) {
                        sh 'git config --global user.email "jenkins@yourdomain.com"'
                        sh 'git config --global user.name "Jenkins"'
                        sh 'git add .'
                        sh 'git commit -m "Auto-format: Applied Black, isort, and autopep8"'
                        sh 'git push origin code_checking'
                    } else {
                        echo 'No formatting changes needed.'
                    }
                }
            }
        }
    }
}