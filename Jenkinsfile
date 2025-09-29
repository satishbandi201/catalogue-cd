pipeline {
    agent  {
        label 'AGENT1'
    }
    environment { 
        appVersion = ''
        REGION = "us-east-1"
        ACC_ID = "838237641372"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
    parameters {
        string(name: 'appVersion', description: 'Image version of the application')
        choice(name: 'deploy_to', choices: ['dev','qa','prod'], description: 'Select the environment to deploy')
   } 
    // Build
    stages {
        stage('Deploy') {
            steps {
                script {
                    withAWS(credentials: 'aws-cred', region: "$REGION") {
                        sh """
                           aws eks update-kubeconfig --region $REGION --name "$PROJECT-${params.deploy_to}"
                           kubectl get nodes
                        """
                    }
                }
            }
        }
    }

    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'Hello Success'
        }
        failure { 
            echo 'Hello Failure'
        }
    }
}