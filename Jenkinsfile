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
                stash name: 'ci-cdp', includes: 'target/*.war'
            }
        }
        stage('Deploy to Tomcat') {
            agent {
                label 'jenkins-slave2'
            }
            steps {
                script {
                    unstash 'ci-cdp'
                    def tomcatDir = "~/apache-tomcat-9.0.44" // Change to the appropriate Tomcat directory
                    def webappsDir = "${tomcatDir}/webapps"
                    sh "sudo rm -rf ${webappsDir}/*.war"
                    sh "sudo mv target/*.war ${webappsDir}/"

                    // Restart Tomcat
                    sh "sudo ${tomcatDir}/bin/catalina.sh stop"
                    sh "sudo ${tomcatDir}/bin/catalina.sh start"
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
        success {
            emailext (
                subject: "Jenkins Job: Success",
                body: "Congratulations team!!! The pipeline has completed successfully.\n\nThanks\nJenkins",
                to: "devopsmail24@gmail.com"
            )
        }
        failure {
            emailext (
                subject: "Jenkins Job: Failure",
                body: "The pipeline has failed.",
                to: "devopsmail24@gmail.com"
            )
        }
    }
}

