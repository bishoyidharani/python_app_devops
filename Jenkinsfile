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
        stage('Docker Login, Tag & Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub_creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]) {
                    sh '''
                        echo "Logging in as $DOCKER_USER"
                        echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin

                        docker tag python-app:latest dharanibishoyi/python-app:latest
                        docker push dharanibishoyi/python-app:latest
                    '''
                }
            }
        }

   }
}
