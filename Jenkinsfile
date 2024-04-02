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
            sh "scp -o StrictHostKeyChecking=no /home/centos/javaweb3/target/WebAppCal-0.0.6.war centos@172.31.14.74:/home/centos/apache-tomcat-7.0.94/webapps/"

            // Restart Tomcat to deploy the application
            sh 'ssh centos@172.31.14.74 /home/centos/apache-tomcat-7.0.94/bin/shutdown.sh || true' // Shutdown Tomcat, ignore errors if it's already stopped
            sh 'ssh centos@172.31.14.74 /home/centos/apache-tomcat-7.0.94/bin/startup.sh' // Start Tomcat
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
