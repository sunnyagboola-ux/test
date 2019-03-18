pipeline {
  agent {
    node {
      label 'master'
    }

  }
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
            ws(dir: 'test')
          }
        }
        stage('IfError') {
          steps {
            echo 'Checkout failed'
            error 'Error'
          }
        }
      }
    }
  }
}