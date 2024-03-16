pipeline {
    agent any
    
    environment {
        SERVER_LOCATION = '/var/www/html/laravel/'
        ENV_FILE = "${SERVER_LOCATION}.env"
        STORAGE_DIRECTORY = "${SERVER_LOCATION}storage/"
        LOG_FILE = "${STORAGE_DIRECTORY}logs/laravel.log"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('pre init') {
            steps {
                sh 'chmod -R 775 ${STORAGE_DIRECTORY}'
                sh "chmod 777 ${LOG_FILE}"
                sh 'chcon -R -t httpd_sys_rw_content_t ${STORAGE_DIRECTORY}'
            }
        }
         
        stage('Deploy to Server') {
            steps {
                sh "cp -r $WORKSPACE/* ${SERVER_LOCATION}"
            }
        }

        stage('post init') {
            steps {
                sh "cp ${SERVER_LOCATION}.env.example ${ENV_FILE}"
                sh "composer install --working-dir=${SERVER_LOCATION}"
                sh "php ${SERVER_LOCATION}artisan key:generate"
                sh "sed -i -e 's/DB_DATABASE=homestead/DB_DATABASE=staging/g' ${ENV_FILE}"
                sh "sed -i -e 's/DB_USERNAME=homestead/DB_USERNAME=yourusername/g' ${ENV_FILE}"
                sh "sed -i -e 's/DB_PASSWORD=secret/DB_PASSWORD=yourpassword/g' ${ENV_FILE}"
                sh "chmod -R 777 ${STORAGE_DIRECTORY}"
                sh "chcon -R -t httpd_sys_rw_content_t ${STORAGE_DIRECTORY}"
                sh "chmod 777 ${LOG_FILE}"
            }
        }
    }
}
