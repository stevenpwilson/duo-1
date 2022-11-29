pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                if (env.BRANCH_NAME == 'development') {
                    sh 'docker build -t stratcastor/duo-backend:latest -t stratcastor/duo-backend:$BUILD_NUMBER .'
                } else {
                    sh "echo 'Build not required!'"
                }
            }
        }
        stage('Push') {
            steps {
                if (env.BRANCH_NAME == 'development') {
                    sh '''
                    docker push stratcastor/duo-backend:latest
                    docker push stratcastor/duo-backend:$BUILD_NUMBER
                    '''
                } else {
                    sh "echo 'Push not required!'"
                }
            }
        }
        stage('Deploy') {
            steps {
                if (env.BRANCH_NAME == 'development') {
                    sh'''
                    kubectl apply -f backend.yaml --namespace=development
                    kubectl apply -f nginx.yaml --namespace=development
                    '''
                } else {
                    sh'''
                    kubectl apply -f backend.yaml --namespace=production
                    kubectl apply -f nginx.yaml --namespace=production
                    '''
                }
            }
        }
    }
}
