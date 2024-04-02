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
        stage('Deploy to Tomcat') {
            agent {
                label 'jenkins-slave2' // Label matching the Jenkins slave node for Tomcat deployment
            }
            steps {
                sshagent(['jenkins-slave2']) {
                    script {
                        def warFilePath = "${WORKSPACE}/javaweb3/target/WebAppCal-0.0.6.war"
                        def tomcatWebappsDir = "/home/centos/apache-tomcat-7.0.94/webapps/"

                        // Check if the WAR file exists
                        if (fileExists(warFilePath)) {
                            // Display where the WAR file is being copied from
                            echo "Copying WAR file from ${warFilePath} to centos@172.31.14.74:${tomcatWebappsDir}"

                            // Move the WAR file to the Tomcat webapps directory
                            sh "scp -o StrictHostKeyChecking=no ${warFilePath} centos@172.31.14.74:${tomcatWebappsDir}"

                            // Restart Tomcat to deploy the application
                            sh "ssh centos@172.31.14.74 /home/centos/apache-tomcat-7.0.94/bin/shutdown.sh || true"
                            sh "ssh centos@172.31.14.74 /home/centos/apache-tomcat-7.0.94/bin/startup.sh"
                        } else {
                            error "WAR file not found at: ${warFilePath}"
                        }
                    }
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
