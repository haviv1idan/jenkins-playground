pipeline {
    agent any
    stages {
        stage ('check python3') {
            steps {
                sh 'python3 --version'
            }
        }
        stage ('check pip') {
            steps {
                sh 'pip --version'
            }
        }
        stage ('run python command') {
            steps {
                sh 'python3 -c "print(\'Hello, World!\')"'
            }
        }
        stage ('create venv and activate it') {
            steps {
                sh 'python3 -m venv .venv'
                sh 'source .venv/bin/activate'
            }
        }
        stage ('install requirements') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
    }
}
