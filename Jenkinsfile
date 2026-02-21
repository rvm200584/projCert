pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rvm200584/projCert.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir("${WORKSPACE}") {      
                sh 'docker build -t my-webapp:latest .'
            }
        }
   }

        stage('Deploy Container') {
            steps {
                sh '''
                docker ps -q --filter "name=php-webapp" | xargs -r docker stop
                docker ps -aq --filter "name=php-webapp" | xargs -r docker rm
                docker run -d --name php-webapp -p 8082:80 php-webapp:latest
                '''
            }
        }
    }

    post {
        failure {
            sh '''
            echo "Build failed! Cleaning up containers..."
            docker ps -q --filter "name=php-webapp" | xargs -r docker stop
            docker ps -aq --filter "name=php-webapp" | xargs -r docker rm
            '''
        }
    }
}
