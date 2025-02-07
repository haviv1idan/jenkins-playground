pipeline {
    agent any
    stages {
        stage ('check python3') {
            steps {
                sh 'python3 --version'
            }
        }
        stage('Ensure pip is installed') {
           steps {
                sh 'python3 -m ensurepip --default-pip'
                sh 'python3 -m pip install --upgrade pip'
            }
        }

        stage ('check pip') {
            steps {
                sh 'pip --version'
            }
        }
    }
}
