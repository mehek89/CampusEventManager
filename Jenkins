pipeline {
    agent any

    tools {
        // Adjust the names according to your Jenkins Tool Configuration
        maven 'Maven-3'
        jdk 'JDK-21'
    }

    environment {
        // Tomcat credentials ID in Jenkins
        TOMCAT_CRED = 'TomcatServer'
    }

    stages {

        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/mehek89/CampusEventManager.git',
                    branch: 'main'
                )
            }
        }

        stage('Build') {
            steps {
                // Clean & package WAR
                bat 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: "${TOMCAT_CRED}", 
                        path: 'CampusEventManager', 
                        url: 'http://localhost:8081'
                    )
                ],
                war: '**/target/CampusEventManager.war',
                debug: true
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed! Check logs.'
        }
    }
}
