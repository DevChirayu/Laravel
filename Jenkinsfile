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

        stage('init') {
        	sh 'cp .env.example .env'
        	sh 'php artisan key:generate'
        	sh "sed -i -e 's/DB_DATABASE=homestead/DB_DATABASE=staging/g' .env"
        	sh "sed -i -e 's/DB_USERNAME=homestead/DB_USERNAME=yourusername/g' .env"
        	sh "sed -i -e 's/DB_PASSWORD=secret/DB_PASSWORD=yourpassword/g' .env"
            sh 'sudo chmod -R 775 storage/'
    	    sh 'sudo chown -R :www-data storage/'
            sh 'sudo chmod 777 storage/logs/laravel.log'
            sh 'sudo chmod -R 755 /var/www/myLaravelApp/storage'
            sh 'sudo chcon -R -t httpd_sys_rw_content_t /var/www/myLaravelApp/storage'
        }
    }
}
