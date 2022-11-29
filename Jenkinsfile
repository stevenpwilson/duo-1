pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker-compose build
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker-compose push
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh'''
                ssh -i "~/.ssh/id_rsa" jenkins@34.130.245.1 << EOF
                rm -rf duo-task
                git clone https://github.com/PCMBarber/duo-task.git
                cd duo-task
                docker-compose down
                docker-compose up -d
                '''
            }
        }
    }
}
