pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'development') {
                    sh 'docker build -t stratcastor/duo-backend:latest -t stratcastor/duo-backend:$BUILD_NUMBER .'
                    } else {
                        sh "echo 'Build not required!'"
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
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
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'development') {
                        sh'''
                        kubectl apply -f backend.yaml --namespace=development
                        kubectl apply -f nginx.yaml --namespace=development
                        kubectl set image deployment/backend --namespace=development backend=stratcastor/duo-backend:latest
                        '''
                    } else if (env.BRANCH_NAME == 'main') {
                        sh'''
                        kubectl apply -f backend.yaml --namespace=production
                        kubectl apply -f nginx.yaml --namespace=production
                        kubectl set image deployment/backend --namespace=production backend=stratcastor/duo-backend:latest
                        '''
                    } else {
                        sh'''
                        echo unrecognised branch'
                        '''
                    }
                }
            }
        }
        stage('Clean Up') { 
            steps {
                sh '''
                docker system prune -a --force
                '''
            }
        }
    }
}

