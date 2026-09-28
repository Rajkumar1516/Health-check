pipeline {
    agent any

    environment {
        // The Public IP of your Ansible Server
        ANSIBLE_SERVER_IP = '13.232.147.60'
        // The path on the Ansible server where files should be copied
        TARGET_DIR        = '/home/ec2-user/'
    }

    stages {
        stage('Transfer Files to Ansible Server') {
            steps {
                echo "Transferring files from Git workspace to the Ansible Server at ${ANSIBLE_SERVER_IP}..."
                
                // Retrieves the AWS PEM private key credential we configured in Jenkins
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-ssh', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    script {
                        // Uses standard secure copy (scp) to move all files to your target directory
                        sh "scp -i ${SSH_KEY} -o StrictHostKeyChecking=no -r ./* ${SSH_USER}@${ANSIBLE_SERVER_IP}:${env.TARGET_DIR}"
                    }
                }
                echo "Files transferred successfully!"
            }
        }
    }
}
