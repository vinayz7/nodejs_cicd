pipeline {
    agent any
       environment {
           DOCKER_IMAGE = 'vinayz7/nodeimg'
       }
    stages {
        stage('clone repository') {
            steps {
                git url: 'https://github.com/vinayz7/nodejs_cicd' , branch: 'main'
            }
        }
        stage('build docker image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        } 
        stage('build image push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'password', usernameVariable: 'username')]) {
                  sh '''
                      docker login -u $username -p $password
                  '''
                  sh 'docker push $DOCKER_IMAGE'
}
            }
        } 
        stage('deploy to kubernetes') {
            steps {
                sh 'kubectl apply -f deploy.yaml'
            }
        } 
        stage('expose service') {
            steps {
                sh 'kubectl apply -f service.yaml'
            }
        } 
        
    }
}

