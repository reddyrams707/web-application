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
        stage('clone the git repository') {
            steps {
                git 'https://github.com/reddyrams707/web-application.git'
            }
        }
         stage('build code using maven') {
            steps {
                sh 'mvn clean install'
            }
        }
         stage('store war file in nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'web-application', classifier: '', file: '/var/lib/jenkins/.m2/repository/com/techworldwithmurali/web-application/1.0-SNAPSHOT/web-application-1.0-SNAPSHOT.war', type: 'war']], credentialsId: 'nexus_credentials', groupId: 'com.techworldwithmurali', nexusUrl: '52.55.7.116:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }
        }
         stage('deploy war file in tomcat') {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'tomcat_credentials', path: '', url: 'http://34.226.142.254:8080')], contextPath: null, war: '**/*.war'
            }
        }
    }
    post {
  always {
    // One or more steps need to be included within each condition's block.
    slackSend channel: 'dev', message: 'Job is completed ', teamDomain: 'devopsteam-zps3902', tokenCredentialId: 'slack_cred-gopi'
  }
}
}
