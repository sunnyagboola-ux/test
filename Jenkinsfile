pipeline {
  agent any
  stages {
    stage('error') {
      steps {
        sh 'ecoo "$workspace"'
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