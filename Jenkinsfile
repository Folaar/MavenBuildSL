pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean install -Dmaven.test.skip=true"
            }
        }
        
        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.war'
            }
        }
        
        stage('Deployment') {
            steps {
                script {
                    def tomcatCreds = credentials('TomcatCreds') // Assuming 'TomcatCreds' is the ID of your Jenkins credentials
                    def tomcatServer = 'http://54.211.79.23:8080/' // Update with your Tomcat server URL
                    
                    deploy adapters: [tomcat( 
                        credentialsId: TomcatCreds.id, 
                        url: tomcatServer
                    )], 
                    war: 'target/*.war',
                    contextPath: 'app'
                }
            }
        }
        
        stage('Notification') {
            steps {
                emailext(
                    subject: "Job Completed",
                    body: "Jenkins pipeline job for Maven build job completed",
                    to: "folar3@gmail.com"
                )
            }
        }
    }
}
