
pipeline {
    agent any
    tools{
        maven "maven3"
    }
    options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '2', numToKeepStr: '3')
  timestamps()
}

    stages {
        stage('clone repository') {
            steps {
                git 'https://github.com/reddyrams707/web-application.git'
            }
        }
        stage('Build the war file') {
            steps {
                sh 'mvn clean install'
            }
        }
         stage('push to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'web-application', classifier: '', file: '/var/lib/jenkins/.m2/repository/com/techworldwithmurali/web-application/1.0-SNAPSHOT/web-application-1.0-SNAPSHOT.war', type: 'war']], credentialsId: 'nexus_credentials', groupId: 'com.techworldwithmurali', nexusUrl: '3.86.154.58:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }
        }
         stage('deploy to tomcat') {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'new_tomcat', path: '', url: 'http://54.175.204.67:8080/')], contextPath: null, war: '**/*.war'
            }
        }

    }

post {
  always {
    slackSend channel: 'dev', message: 'build completed', teamDomain: 'devopsteam-zps3902', tokenCredentialId: 'slack_cred-gopi'
  }
}


}
