pipeline {
  agent any

  environment {
    FLY_TOKEN = ('FLY_API_TOKEN')
  }


  tools {
    nodejs "nodejs-18"
  }

  triggers{
    githubPush()
  }
 
  stages {
    stage ('Pintar mi credencial'){
      steps{
        echo 'Esta es mi credencial: $FLY_API_TOKEN'
        }
    }

    stage ('Install dependencies') {
      steps {
        echo 'Installing'
        sh 'npm install'
      } 
    }
    stage ('Run test') {
      steps {
        echo 'Running test'
        sh 'npm run test'
      }      
    }
    stage ('Install fly.io') {
        steps {
          echo 'Installing Fly.io'
          withCredentials([string(credentialsId: 'FLY_API_TOKEN', variable: 'FLY_API_TOKEN')]) {
          sh '''
            curl -L https://fly.io/install.sh | sh
            export FLYCTL_INSTALL="/var/jenkins_home/.fly"
            export PATH="$FLYCTL_INSTALL/bin:$PATH"
            fly auth token $FLY_API_TOKEN
          '''
        }
      }
  
  }
}   

     