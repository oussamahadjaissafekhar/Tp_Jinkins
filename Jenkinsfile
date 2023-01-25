pipeline {
  agent any
  stages {
    
    stage('test'){
        steps{
          bat 'gradlew test'
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
          bat "gradlew sonarqube"
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
          bat "gradlew publish" 
      } 
    }
    
    stage("Notification") {
      steps {
          notifyEvents message: 'Congrats , success', token: 'hNmvPSDaVAbXJ39nb-Yscop9bwU7hA6Q'
      }
    }
}
  post {
        failure {
            mail bcc: '', body: '''Error occured !!!''', cc: '', from: '', replyTo: '', subject: 'Pipleline failiure', to: 'jo_hadjaissafekhar@esi.dz'
        }
  }

}
