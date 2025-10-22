pipeline {
    agent any

    environment {
        // Set the Maven and JDK tools installed on Jenkins
        MAVEN_HOME = tool name: 'Maven3', type: 'maven'
        JAVA_HOME  = tool name: 'JDK21', type: 'jdk'

        // Tomcat configuration
        TOMCAT_URL = "http://localhost:8081"          // URL to Tomcat manager
        TOMCAT_CRED = "TomcatServer"                  // Jenkins credentials ID for Tomcat
        APP_NAME   = "CampusEventManager"            // Context path for your app
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mehek89/CampusEventManager.git',
                    credentialsId: '519449d1-d9dd-49fa-9299-a465a667a8f5'
            }
        }

        stage('Build') {
            steps {
                withEnv(["PATH+MAVEN=${MAVEN_HOME}/bin", "JAVA_HOME=${JAVA_HOME}"]) {
                    bat "mvn clean package"
                }
            }
        }

        stage('Deploy') {
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
            echo "Build and deployment successful! Your app should be live at ${TOMCAT_URL}/${APP_NAME}"
        }
        failure {
            echo "Build or deployment failed! Check the console logs."
        }
    }
}
