pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build demo-app'
        sh 'sh run_build_script.sh'
      }
    }

    stage('Test') {
      parallel {
        stage('Linux Tests') {
          steps {
            echo 'Linux tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('Windows tests') {
          steps {
            echo 'Windows tests'
          }
        }

      }
    }

    stage('Deploy Staging') {
      parallel {
        stage('Deploy Staging') {
          steps {
            echo 'Deploy to staging environment'
            input 'Ok to deploy to production?'
          }
        }

        stage('error') {
          steps {
            echo 'Abr'
          }
        }

      }
    }

    stage('Deploy Production') {
      steps {
        echo 'Deploy to Production'
        sh 'java -version'
      }
    }

  }
  tools {
    jdk 'java11'
  }
  post {
    always {
      archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
    }

    failure {
      mail(to: 'ci-team@example.com', subject: "Failed Pipeline ${currentBuild.fullDisplayName}", body: " For details about the failure, see ${env.BUILD_URL}")
    }

  }
}