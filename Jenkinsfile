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
                    def tomcatCreds = credentials('TomcatCreds')
                    def tomcatServer = 'http://54.174.85.210:8080/'
                    
                    deploy adapters: [tomcat9(
                        credentialsId: tomcatCreds.id,
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
