pipeline {
    agent any
    tools {
        nodejs 'node'
    }
    stages {
        stage('Build Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                                      usernameVariable: 'USER',
                                                      passwordVariable: 'PASS')]) {
                        sh '''
                        docker build -t kelzceana/react-app_ec2:1.0 .
                        echo $PASS | docker login -u $USER --password-stdin
                        docker push kelzceana/react-app_ec2:1.0
                        '''
                                                      }
                }
            }
        }
        stage('Deploy Image to EC2') {
            steps {
                script {
                    def dockerCmd = 'docker run -d -p 3000:3080 kelzceana/react-app_ec2:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@52.204.217.143 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
