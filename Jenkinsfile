pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t stratcastor/duo-backend:latest -t stratcastor/duo-backend:$BUILD_NUMBER .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push stratcastor/duo-backend:latest
                docker push stratcastor/duo-backend:$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh'''
                kubectl apply -f backend.yaml
                kubectl apply -f nginx.yaml
                '''
            }
        }
    }
}
