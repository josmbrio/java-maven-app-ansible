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
                            sh 'scp $keyfile root@188.166.4.98:~/josmbrio-key.pem'
                        }
                    }
                }
            }
        }
        stage("execute ansible playbook") {
            steps {
                script {
                    echo "calling ansible playbook to configure ec2 instances"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "188.166.4.98"
                    remote.allowAnyHosts = true
                    
                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-ssh-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            //sh 'ssh -i $keyfile root@188.166.4.98 "ansible-playbook my-playbook.yaml"'
                            remote.user = user
                            remote.identityFile = keyfile
                            sshCommand remote: remote, command: 'ansible-playbook my-playbook.yaml'
                        }
                    
                }
            }
        }
    }   
}