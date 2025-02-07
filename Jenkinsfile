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
    }
}
