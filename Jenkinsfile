pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://localhost:9090/manager/text'
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'admin123'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/digitalkfmw/cicdpipeline.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
                curl -v -u %TOMCAT_USER%:%TOMCAT_PASS% -T target/cicd-webapp.war %TOMCAT_URL%/deploy?path=/cicd-webapp
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Successful! Check your app at http://localhost:9090/cicd-webapp'
        }
        failure {
            echo 'Build, Test, or Deployment Failed!'
        }
    }
}