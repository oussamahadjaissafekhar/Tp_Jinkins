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
        withSonarQubeEnv('sonare') {
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
          bat "gradlew build"
          bat "gradlew javadoc"
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
          notifyEvents message: 'Good evening <b>ISLAM</b>', token: 'hNmvPSDaVAbXJ39nb-Yscop9bwU7hA6Q'
      }
    }
}
  post {
        failure {
            mail bcc: '', body: '''Error occured !''', cc: '', from: '', replyTo: '', subject: 'Pipleline failiure', to: 'jo_hadjaissafekhar@esi.dz'
        }
  }

}
