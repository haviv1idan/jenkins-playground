pipeline {
    agent any

    stages {
        stage('Setup') {
            steps {
                script {
                    echo 'Setting up virtual environment...'
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate && pip install --upgrade pip'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh '. venv/bin/activate && pip install -r requirements.txt'
                }
            }
        }

        stage('Linting') {
            steps {
                script {
                    sh '. venv/bin/activate && flake8 --max-line-length=100 .'
                }
            }
        }

        stage('Testing') {
            steps {
                script {
                    sh '. venv/bin/activate && pytest --junitxml=report.xml'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    echo 'Cleaning up workspace...'
                    sh 'rm -rf venv'
                }
            }
        }
    }

    post {
        always {
            junit 'report.xml'
        }
        success {
            echo 'Python code quality check passed! ✅'
        }
        failure {
            echo 'Python code quality check failed! ❌'
        }
    }
}
