pipeline {
    agent {
        node {
            label 'webserver'
        }
    }

    stages {
        stage('Build') {
            steps {
              sh '''
                docker build -t abdulwahabshukri/jenkins-frontend:jenkins-${BUILD_NUMBER} .
              '''
            }
        }
        stage('Release') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 
                sh '''
                docker login -u $USERNAME -p $PASSWORD
                docker push abdulwahabshukri/jenkins-frontend:jenkins-${BUILD_NUMBER}
                '''
              }
            }
        }
        stage('deploy') {
            steps {
                sh '''
                docker stop jenkins-frontend || true
                docker rm -f jenkins-frontend || true
                docker run -p3000:80 -d --name jenkins-frontend abdulwahabshukri/jenkins-frontend:jenkins-${BUILD_NUMBER}
                '''
            }
        }
    }
}