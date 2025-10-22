pipeline {
    agent any

    tools {
        maven 'Maven3'   // Use exact Maven tool name
        jdk 'JDK21'      // Use exact JDK tool name
    }

    environment {
        TOMCAT_CRED = 'tomcat-cred'
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
                bat 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: "${TOMCAT_CRED}",
                        path: 'CampusEventManager',
                        url: 'http://localhost:8080'
                    )
                ],
                war: '**/target/CampusEventManager.war',
                verbose: true
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
