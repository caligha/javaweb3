pipeline {
    agent {
        label 'jenkins-slave1' // Label matching the Jenkins slave node for build
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your GitHub repository
                git 'https://github.com/caligha/javaweb3.git'
            }
        }

        stage('Build') {
            steps {
                // Build your web application using Maven
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                // Run tests for your web application
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            agent {
                label 'jenkins-slave2' // Label matching the Jenkins slave node for Tomcat deployment
            }
            steps {
                // Move the WAR file to the Tomcat webapps directory
                sh 'mv target/*.war ~/apache*/webapps/'

                // Restart Tomcat to deploy the application
                sh 'ssh user@172.31.14.74 /path/to/tomcat/bin/shutdown.sh'
                sh 'ssh user@172.31.14.74 /path/to/tomcat/bin/startup.sh'
            }
        }
    }

    post {
        success {
            echo 'Congratulations! The pipeline has completed successfully.'
            // You can add additional steps or actions here if needed
        }
    }
}
