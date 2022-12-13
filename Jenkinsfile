pipeline {
    agent any
    environment {
        ANSIBLE_SERVER = 188.166.4.98
    }
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all neccessary files to ansible control node"
                    sshagent(['ansible-ssh-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER}:/root/"
                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            sh 'scp $keyfile root@$ANSIBLE_SERVER:~/josmbrio-key.pem'
                        }
                    }
                }
            }
        }
        stage("execute ansible playbook") {
            steps {
                script {
                    echo "calling ansible playbook to configure ec2 instances"
                    //def remote = [:]
                    //remote.name = "ansible-server"
                    //remote.host = env.ANSIBLE_SERVER
                    //remote.allowAnyHosts = true
                    
                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-ssh-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            sh 'ssh -i $keyfile root@${ANSIBLE_SERVER} "prepare-ansible-server.sh"'
                            sh 'ssh -i $keyfile root@${ANSIBLE_SERVER} "ansible-playbook my-playbook.yaml"'
                            //remote.user = user
                            //remote.identityFile = keyfile
                            //sshScript remote: remote, command: "prepare-ansible-server.sh"
                            //sshCommand remote: remote, command: "ansible-playbook my-playbook.yaml"
                        }
                    
                }
            }
        }
    }   
}