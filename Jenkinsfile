pipeline {
    agent any
    
    environment {
        SERVER_LOCATION = '/var/www/laravel/'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
         
        stage('Deploy to Server') {
            steps {
                sh "cp -r $WORKSPACE/* $SERVER_LOCATION"
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    // Run SonarScanner
                    script {
                        def scannerHome = tool 'SonarScanner';
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
    
    post {
        success {
            slackSend color: 'good', message: "Build succeeded! :white_check_mark:", tokenCredentialId: 'slack-token', channel: 'jenkins-build-deployement'
        }
        failure {
            slackSend color: 'danger', message: "Build failed! :x:", tokenCredentialId: 'slack-token', channel: 'jenkins-build-deployement'
        }
    }
}