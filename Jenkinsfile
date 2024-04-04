

User
pipeline {
    agent {
        label 'jenkins-node1'
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
                stash name: 'jenkins-pipeline', includes: 'target/*.war'
            }
        }
        stage('Deploy to Tomcat') {
            agent {
                label 'jenkins-node2'
            }
            steps {
                script {
                    unstash 'jenkins-pipeline'
                    sh "sudo rm -rf ~/apache*/webapps/*.war"
                    sh "sudo mv target/*.war ~/apache*/webapps/"

                    // Restart Tomcat
                    sh "sudo ~/apache-tomcat-7.0.94/bin/catalina.sh stop"
                    sh "sudo ~/apache-tomcat-7.0.94/bin/catalina.sh start"
                }
            }
        }
    }
    post {
        always {
            echo 'Congratulations! The pipeline has completed successfully.'
        }
        failure {
            echo 'Pipeline failed!'
            // Add additional actions for failure handling if needed
        }
    }
}