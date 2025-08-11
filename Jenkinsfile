pipeline {
    agent any
    
    environment {
         WEBSERVER = "Nginx"
    }
    
    stages {
        stage('Create directory for the WEB Application')
        {
            steps {
                //Fisrt, drop the directory if exists
                sh 'rm -rf $(pwd)/app-web'
                //Create the directory
                sh 'mkdir $(pwd)/app-web'
                
            }
        }
        stage('Drop the container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f app-web'
            }
        }
        // Apache Webserver
        stage('Create the Apache container') {
            when {
                 environment name: 'WEBSERVER', value: 'Apache'
            }
            steps {
            echo 'Creating the container...'
            sh 'docker run -dit --name app-web -p 9100:80  -v $(pwd)/app-web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        //Nginx webserver
        stage('Create the Nginx container') {
            when {
                 environment name: 'WEBSERVER', value: 'Nginx'
            }
            steps {
            echo 'Creating the container...'
            sh 'docker run -dit --name app-web -p 9100:80  -v $(pwd)/app-web:/usr/share/nginx/html nginx'
         
            }
        }

        
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
                sh 'cp -r web/* $(pwd)/app-web'
            }
        }
    }

    post {
        always {
            echo 'These steps are always executed'   
        }
      
        success {
        // One or more steps need to be included within each condition's block.
          echo 'the deployment has worked'
          archiveArtifacts allowEmptyArchive: true, artifacts: 'web/*', followSymlinks: false
          cleanWs()         

       }
       failure {
        // One or more steps need to be included within each condition's block.
        echo 'An error has ocurred'       
       }
    }
}
