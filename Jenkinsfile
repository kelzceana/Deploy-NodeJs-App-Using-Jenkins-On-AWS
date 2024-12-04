pipeline {
    agent any
    tools {
        nodejs 'node'
    }
    stages {
        stage('Pull Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                                      usernameVariable: 'USER',
                                                      passwordVariable: 'PASS')]) {
                                                        sh 'docker pull kelzceana/reactapp:1.0'
                                                       }
                }
            }
        }
        stage('Deploy Image to EC2') {
            steps {
                script {
                    def dockerCmd = 'docker run -d -p 3000:3080 kelzceana/reactapp:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@54.221.164.84 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
