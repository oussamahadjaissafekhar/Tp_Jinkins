pipeline {
  agent any
  stages {
    
    stage('test'){
        steps{
          bat 'gradle test'
          archiveArtifacts 'build/test-results/'
          cucumber reportTitle: 'Report',
                   fileIncludePattern: 'target/report.json',
                   trendsLimit: 10
          junit 'build/test-results/test/TEST-Matrix.xml'
        }
    }
    
    stage ('Code Analysis') {
      steps{
        withSonarQubeEnv('sonar') {
          bat "gradle sonarqube"
        }
      }
    }
    
    stage("Code Quality") {
      steps {
          waitForQualityGate abortPipeline: true
      }
    }
    
    stage('Build') {
      steps {
          bat "gradle build"
          bat "gradle javadoc"
          archiveArtifacts 'build/libs/*.jar'
          archiveArtifacts 'build/docs/'
      }
    }
    
    stage("Deploy") {
      steps {
          bat "gradle publish" 
      } 
    }
    
    stage("Notification") {
      steps {
          notifyEvents message: 'Good evening <b>ISLAM</b>', token: '5S-0a-3fKCME0wr9p5riirUmtHrggCiX'
      }
    }
}
  post {
        failure {
            mail bcc: '', body: '''Eror DANGER AAAAHHHHH !!''', cc: '', from: '', replyTo: '', subject: 'Pipleline failiure', to: 'jy_ghodbane@esi.dz'
        }
  }

}
