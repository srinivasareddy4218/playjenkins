pipeline {

  agent { 
    
    label 'kubepod'
  }
  
  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/justmeandopensource/playjenkins.git', branch:'test-deploy-stage'
      }
    }

    stage('Deploy App') {
      steps {
        script {
        sh "kubectl apply -f nginx.yaml "
        }
      }
    }

  }

}
