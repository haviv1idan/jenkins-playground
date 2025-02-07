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
    }
}
