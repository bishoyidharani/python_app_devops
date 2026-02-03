pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/bishoyidharani/python_app_devops'
            }
        }
         stage('build') {
            steps {
                sh 'python -m py_compile app/app.py'
            }
        }
        stage('test'){
            steps{
                sh 'python -m unittest discovery test '
            }
        }
        stage('docker image'){
            steps{
                sh 'docker build -t python-app:latest .'
            }
        }
        stage('docker run'){
            steps{
                sh 'docker run -rm python-app:latest'
            }
        }
    }
}
