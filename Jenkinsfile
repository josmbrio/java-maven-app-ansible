pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all neccessary files to ansible control node"
                    sshagent(['ansible-ssh-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@188.166.4.98:/root/"
                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            sh "scp ${keyfile} root@188.166.4.98:~/josmbrio-key.pem"
                        }
                    }
                }
            }
        }
    }   
}