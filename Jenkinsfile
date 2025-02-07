pipeline {
    agent any
    
    environment {
        GIT_CREDENTIALS_ID = 'github-pat'  // Replace with Jenkins credential ID
        GIT_BRANCH = 'code_checking'  // Change to the appropriate branch
    }
    
    stages {
        
        stage('Install Dependencies') {
            steps {
                sh 'pip install black isort autopep8'
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
