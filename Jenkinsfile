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
                    sh "docker push bsever1/tomcat:${env.BRANCH_NAME}"
                }
            }
        }
        stage('Deploy') {
            when {
                branch '1.0'
            }
            steps {
                sshagent(['remote-server']) {
                    sh 'ssh remote-user@remote-server "docker pull bsever1/tomcat:1.0 && docker run -d -p 8081:8080 bsever1/tomcat:1.0"'
                }
            }
        }
        // Add additional deploy stages for each branch as needed
    }
}
