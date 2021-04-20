pipeline {
    environment {
        imagename = "h04x3r/exam-app"
        app_url = "http://10.0.0.81:88"
    }
    agent any
    stages {
        stage('Clone repo') {
            steps {
                echo 'Cloning..'
                sh """
                    git clone https://github.com/h04x/ansible-exam2.git .
                """
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'h04x3r-dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    ansiblePlaybook( 
                            playbook: 'run.yml',
                            inventory: 'hosts', 
                            credentialsId: 'deploy',
                            extras: '--ssh-extra-args="-o StrictHostKeyChecking=no" -e hub_login=${USERNAME} -e hub_password=${PASSWORD} -e imagename=${imagename}')
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    httpRequest app_url
                }
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}
