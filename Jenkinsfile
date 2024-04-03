pipeline {
    agent {
        label 'jenkins-slave1'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/caligha/javaweb3.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy to Tomcat') {
            agent {
                label 'jenkins-slave2'
            }
            steps {
                script {
                    // Add appropriate error handling
                    unstash 'jenkins CI-CD'
                    sh "sudo rm -rf ~/apache*/webapp/*.war"
                    sh "sudo mv target/*.war ~/apache*/webapps/"

                    // Restart Tomcat
                    sh "sudo ~/apache-tomcat-7.0.94/bin/catalina.sh stop"
                    sh "sudo ~/apache-tomcat-7.0.94/bin/catalina.sh start"
                }
            }
        }
    }
    post {
        success {
            echo 'Congratulations! The pipeline has completed successfully.'
        }
        failure {
            echo 'Pipeline failed!'
            // Add additional actions for failure handling if needed
        }
    }
}


