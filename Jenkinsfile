pipeline {
    agent any
    
    tools{
        maven "maven 3.9.6"
    }

    stages {
        stage('Clone the Repository') {
            steps {
                git credentialsId: 'GitHub_credential', url: 'https://github.com/reddyrams707/web-application.git'
            }
        }
        
        stage('Build the code') {
            steps {
                sh 'mvn clean package'

            }
        }
        
        stage('push the war file in  nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'web-application', classifier: '', file: '/var/lib/jenkins/workspace/build and deploy the war file in Tomcat/target/web-application war file.war', type: 'war']], credentialsId: 'nexus_credential', groupId: 'com.techworldwithmurali', nexusUrl: '35.154.226.252:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }
        }
        
         stage('deploy the war file to tomcat') {
            steps {
               deploy adapters: [tomcat7(credentialsId: 'tomcat_credentials', path: '', url: 'http://13.201.60.105:8080/')], contextPath: null, war: '**/*.war'
            }
        }
    }
    
    post {
  always {
    // One or more steps need to be included within each condition's block.
    slackSend channel: 'dev', message: 'build compelted', teamDomain: 'devopsteam-zps3902', tokenCredentialId: 'slack-for-dev'
  }
}
}
