pipeline {
    agent any
    stages {

        stage('DeployToProduction') {
            
            steps {
                input 'Deploy to Production?'
                milestone(1)
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    script {
                        sh "sshpass -p '$USERPASS'  ssh -o StrictHostKeyChecking=no $USERNAME@192.168.10.12 \"docker pull ostap/train-schedule:latest\""
                        try {
                            sh "sshpass -p '$USERPASS' ssh -o StrictHostKeyChecking=no $USERNAME192.168.10.12 \"docker stop train-schedule\""
                            sh "sshpass -p '$USERPASS' ssh -o StrictHostKeyChecking=no $USERNAME192.168.10.12 \"docker rm train-schedule\""
                        } catch (err) {
                            echo: 'caught error: $err'
                        }
                        sh "sshpass -p '$USERPASS' ssh -o StrictHostKeyChecking=no $USERNAME@192.168.10.12 \"docker run --restart always --name train-schedule -p 8080:8080 -d ostap/train-schedule:latest\""
                    }
                }
            }
        }
    }
}
