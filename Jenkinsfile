pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    environment {
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
                war: '**/target/CampusEventManager.war'
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed! Check Jenkins console logs.'
        }
    }
}
