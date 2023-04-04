pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t bsever/tomcat:${env.BRANCH_NAME} .'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh "docker push bsever/tomcat:${env.BRANCH_NAME}"
                }
            }
        }
        stage('Deploy') {
            when {
                branch '3.0'
            }
            steps {
                sshagent(['remote-server']) {
                    sh 'ssh remote-user@remote-server "docker pull bsever/tomcat:3.0 && docker run -d -p 8080:8080 bsever/tomcat:3.0"'
                }
            }
        }
        // Add additional deploy stages for each branch as needed
    }
}
