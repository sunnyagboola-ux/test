pipeline {
  agent any
  stages {
    stage('error') {
      steps {
        sh 'ecoo "$workspace"'
        catchError() {
          echo 'command not found'
        }

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