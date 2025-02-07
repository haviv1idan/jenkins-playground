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
                sh 'pip install --user --upgrade pip --no-cache-dir'
                sh 'pip install --no-cache-dir --user -r requirements.txt'
            }
        }   
        stage('Run Code Formatters') {
            steps {
                sh 'black .'
                sh 'isort .'
                sh 'autopep8 --in-place --recursive .'
            }
        }
        
        stage('Check for Changes') {
            steps {
                script {
                    def changes = sh(script: 'git status --porcelain', returnStdout: true).trim()
                    if (changes) {
                        sh 'git config --global user.email "jenkins@yourdomain.com"'
                        sh 'git config --global user.name "Jenkins"'
                        sh 'git add .'
                        sh 'git commit -m "Auto-format: Applied Black, isort, and autopep8"'
                        sh 'git push origin ${GIT_BRANCH}'
                    } else {
                        echo 'No formatting changes needed.'
                    }
                }
            }
        }
    }
}