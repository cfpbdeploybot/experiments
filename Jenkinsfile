pipeline {
  agent any
  stages {
    stage('hello') {
      steps {
        parallel(
          "hello": {
            echo 'Hello World'
            
          },
          "Git commit": {
            script {
              gitCommit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
              // short SHA, possibly better for chat notifications, etc.
              shortCommit = gitCommit.take(6)
              println gitCommit
              println shortCommit
            }
            
            
          }
        )
      }
    }
    stage('Test and Package') {
      steps {
        parallel(
          "Package": {
            sleep 3
            sh """
              echo ${gitCommit} > gigantic_binary.txt
              echo "Packaging finished"
            """
            archiveArtifacts(artifacts: 'gigantic*.*', fingerprint: true, onlyIfSuccessful: true)
            
          },
          "Security Tests": {
            sleep 2
            sh 'echo "Security Tests Finished"'
            
          },
          "Unit Tests": {
            sleep 1
            sh 'echo "Unit Tests Finished"'
            
          }
        )
      }
    }
    stage('Push to S3') {
      steps {
        parallel(
          "Push to S3": {
            sh 'echo "Pushing git commit to S3" '
            
          },
          "Push to S3 Groovy": {
            script {
              println "Pushing ${gitCommit} to S3"
            }
            
            
          }
        )
      }
    }
  }
}