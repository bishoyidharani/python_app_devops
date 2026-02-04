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
                sh 'python3 -m py_compile app/app.py'
            }
        }
        stage('test'){
            steps{
                sh 'python3 -m unittest discover test '
            }
        }
        stage('docker image'){
            steps{
                sh 'docker build -t python-app:latest .'
            }
        }
        stage('docker run'){
            steps{
                sh 'docker run --rm python-app:latest'
            }
        }
        stage('tag and push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub_creds',
                        usernameVariable: 'dockerhub',
                        passwordVariable: 'dockerhubPassword'
                    )
                ]) {
                    sh '''
                        echo "$dockerhubPassword" | docker login -u "$dockerhubuser" --password-stdin
                        docker tag python-app:latest dharanibishoyi/python-app:latest
                        docker push dharanibishoyi/python-app:latest
                    '''
                }
            }
        }
   }
}
