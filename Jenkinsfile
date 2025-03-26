pipeline {
    agent any

    environment {
        DEBIAN_FRONTEND = 'noninteractive' // Disable interactive prompts
    }
    
    stages {
        stage('Git clone') {
            steps {
                git credentialsId: 'GithubPAToken', url: 'https://github.com/MVLogix/Nginx-Laravel.git', branch: 'master'
            }
        }
        
        
        
        stage('Deploy Locally') {
            steps {
                script {
                    
                    // Set proper permissions for laravelapp 
                    sh 'sudo chown -R www-data.www-data /var/www/phplaravelapp/storage'
		    sh 'sudo chown -R www-data.www-data /var/www/phplaravelapp/bootstrap/cache'
					
		    sh 'cd /etc/nginx/sites-enabled'
		    sh 'sudo rm /etc/nginx/sites-enabled/phplaravelapp'
		    sh 'cd /etc/nginx/sites-available'
		    sh 'sudo ln -s /etc/nginx/sites-available/phplaravelapp  /etc/nginx/sites-enabled/'
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
