pipeline {
    agent any
    
    environment {
        SERVER_LOCATION = '/var/www/html/laravel/'
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
    }
}