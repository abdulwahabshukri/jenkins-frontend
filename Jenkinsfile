pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
              sh '''
                docker build -t abdulwahabshukri/ttd-frontend:jenkins-${GITHUB_RUN_ID} .
              '''
            }
        }
        stage('Release') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 
                sh '''
                docker login -u $USERNAME -p $PASSWORD
                docker push abdulwahabshukri/ttd-frontend:jenkins-${GITHUB_RUN_ID}
                '''
              }
            }
        }
        stage('deploy') {
            steps {
                sh '''
                docker stop ttd-frontend || true
                docker rm -f ttd-frontend || true
                docker run -p3000:80 -d --name ttd-frontend abdulwahabshukri/ttd-frontend:jenkins-${GITHUB_RUN_ID}
                '''
            }
        }
    }
}