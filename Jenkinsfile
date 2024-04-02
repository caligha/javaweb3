pipeline {
    agent {
        label 'jenkins-slave1' // Label matching the Jenkins slave node for build
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your GitHub repository
                git branch: 'master', url: 'https://github.com/caligha/javaweb3.git'
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
                sshagent(['jenkins-slave2']) {
                    // Move the WAR file to the Tomcat webapps directory
                    sh "scp -o StrictHostKeyChecking=no webapp/target/webapp.war centos@172.31.14.74:/home/centos/webapps/"

                    // Restart Tomcat to deploy the application
                    sh 'ssh user@172.31.14.74 /path/to/tomcat/bin/shutdown.sh || true' // Shutdown Tomcat, ignore errors if it's already stopped
                    sh 'ssh user@172.31.14.74 /path/to/tomcat/bin/startup.sh' // Start Tomcat
                }
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
