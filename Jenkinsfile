pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    environment {
        TOMCAT_CRED = 'TomcatServer'
        APP_NAME = 'CampusEventManager'  // context path
        TOMCAT_URL = 'http://localhost:8081'
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

        stage('Undeploy Old App') {
            steps {
                script {
                    // Try to undeploy; ignore failures if app does not exist
                    try {
                        deploy adapters: [
                            tomcat9(
                                credentialsId: "${TOMCAT_CRED}",
                                path: "${APP_NAME}",
                                url: "${TOMCAT_URL}"
                            )
                        ],
                        action: 'undeploy'
                    } catch (Exception e) {
                        echo "No existing app to undeploy, continuing..."
                    }
                }
            }
        }

        stage('Deploy New WAR') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: "${TOMCAT_CRED}",
                        path: "${APP_NAME}",
                        url: "${TOMCAT_URL}"
                    )
                ],
                war: '**/target/CampusEventManager.war'
            }
        }
    }

    post {
        success {
            echo 'Build and Redeploy Successful!'
        }
        failure {
            echo 'Build or Redeploy Failed! Check Jenkins console logs.'
        }
    }
}
