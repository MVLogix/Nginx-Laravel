pipeline {
    agent any

    
    stages {
        stage('Git clone') {
            steps {
                git credentialsId: 'GithubPAToken', url: 'https://github.com/MVLogix/Nginx-Laravel.git', branch: 'master'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'sudo apt install nginx mysql-server php php-fpm php-mbstring php-xml php-bcmath php-curl zip unzip -y'

                    sh 'sudo curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/bin --filename=composer'

                    sh 'sudo systemctl start nginx.service'

                    sh 'sudo systemctl enable nginx.service'
                }
            }
        }
        
        
        stage('Deploy Locally') {
            steps {
                script {
				
		    // create laravel app 
		    sh 'sudo composer create-project --prefer-dist laravel/laravel phplaravelapp'
                    
                    // Set proper permissions for laravelapp 
                    sh 'sudo chown -R www-data.www-data /var/www/phplaravelapp/storage'
		    sh 'sudo chown -R www-data.www-data /var/www/phplaravelapp/bootstrap/cache'
					
		    sh 'cd /etc/nginx/sites-available'
		    sh 'sudo ln -s /etc/nginx/sites-available/phplaravelapp /etc/nginx/sites-enabled/'
		    sh 'sudo nginx -t'
       		    sh 'sudo systemctl restart nginx.service'
                }
            }
        }
    }
    
    post {
        always {
            // Clean up steps
            echo 'Cleaning up...'
        }
        success {
            // Notify success
            echo 'The pipeline ran successfully!'
        }
        failure {
            // Notify failure
            echo 'There was an error in the pipeline.'
        }
    }
}
