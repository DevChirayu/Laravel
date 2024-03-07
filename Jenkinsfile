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

        stage('pre init') {
            steps {
                sh 'chmod -R 775 /var/www/html/laravel/storage/'
                sh 'chmod 777 /var/www/html/laravel/storage/logs/laravel.log'
                sh 'chcon -R -t httpd_sys_rw_content_t /var/www/html/laravel/storage'
            }
        }
         
        stage('Deploy to Server') {
            steps {
                sh "cp -r $WORKSPACE/* $SERVER_LOCATION"
            }
        }

        stage('post init') {
            steps {
                sh 'cp .env.example .env'
                sh 'composer install'
                sh 'php artisan key:generate'
                sh "sed -i -e 's/DB_DATABASE=homestead/DB_DATABASE=staging/g' .env"
                sh "sed -i -e 's/DB_USERNAME=homestead/DB_USERNAME=yourusername/g' .env"
                sh "sed -i -e 's/DB_PASSWORD=secret/DB_PASSWORD=yourpassword/g' .env"
                sh 'chmod 777 storage/logs/laravel.log'
                sh 'chmod -R 755 /var/www/html/laravel/storage'
                sh 'chcon -R -t httpd_sys_rw_content_t /var/www/html/laravel/storage'
            }
        }
    }
}
