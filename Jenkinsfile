pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/development') {
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
                    if (env.GIT_BRANCH == 'origin/development') {
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
                    if (env.GIT_BRANCH == 'origin/development') {
                        sh'''
                        kubectl apply -f backend.yaml --namespace=development
                        kubectl apply -f nginx.yaml --namespace=development
                        kubectl rollout restart deployment backend --namespace=development
                        '''
                    } else if (env.GIT_BRANCH == 'origin/main') {
                        sh'''
                        kubectl apply -f backend.yaml --namespace=production
                        kubectl apply -f nginx.yaml --namespace=production
                        kubectl rollout restart deployment backend --namespace=production
                        '''
                    } else {
                        sh'''
                        echo "unrecognised branch"
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

