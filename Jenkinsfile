pipeline {
  agent any

  environment {
    FLY_TOKEN = credentials('FLY_API_TOKEN')
  }


  tools {
    nodejs "nodejs-18"
  }

  triggers{
    githubPush()
  }
 
  stages {
    stage ('Install fly.io') {
      steps {
        echo 'Installing Fly.io'
        //withCredentials([string(credentialsId: 'FLY_API_TOKEN', variable: 'FLY_API_TOKEN')]) {
        sh '''
          curl -L https://fly.io/install.sh | sh
          FLYCTL_INSTALL="/var/jenkins_home/.fly"
          PATH="$FLYCTL_INSTALL/bin:$PATH"
          echo "export PATH=\$PATH" >> $HOME/.bashrc
          flyctl auth token ${FLY_TOKEN}
          '''
        }
      }
    
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
    stage ('Deploy to Fly.io') {
        steps {
          echo 'Deploying to fly.io'
          sh 'flyctl auth token $FLY_API_TOKEN'
          sh '$PATH flyctl deploy --app practicafinaljenkins --remote-only'          
        }
      }
  }
}   

     