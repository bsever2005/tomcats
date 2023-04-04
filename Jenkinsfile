pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t bsever1/tomcat:${env.BRANCH_NAME} .'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh "docker push bsever1/tomcat:${env.BRANCH_NAME}"
                }
            }
        }
        stage('Deploy') {
            when {
                branch '2.0'
            }
            steps {
                sshagent(['remote-server']) {
                    sh 'ssh remote-user@remote-server "docker pull bsever1/tomcat:2.0 && docker run -d -p 8082:8080 bsever/tomcat:2.0"'
                }
            }
        }
        // Add additional deploy stages for each branch as needed
    }
}
