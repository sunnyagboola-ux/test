pipeline {
  agent any
  stages {
    stage('error') {
      steps {
        sh 'echo $workspace'
        catchError() {
          echo 'echo command not found'
        }

        echo 'command not found'
      }
    }
    stage('Msg') {
      parallel {
        stage('Msg') {
          steps {
            echo 'Checkout finished'
            catchError()
          }
        }
        stage('IfError') {
          steps {
            echo 'Checkout failed'
          }
        }
      }
    }
  }
}