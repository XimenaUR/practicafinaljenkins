pipeline {
  agent any



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
        withCredentials([string(credentialsId: 'FLY_TOKEN', variable: 'FLY_API_TOKEN')]) {
        sh '''
          curl -L https://fly.io/install.sh | sh
          FLYCTL_INSTALL="/var/jenkins_home/.fly"
          PATH="$FLYCTL_INSTALL/bin:$PATH"
          #echo "export PATH=\$PATH" >> $HOME/.bashrc
          flyctl auth token ${FLY_API_TOKEN}
          '''
        }
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
          sh '''
          FLYCTL_INSTALL="/var/jenkins_home/.fly"
          PATH="$FLYCTL_INSTALL/bin:$PATH"
          flyctl auth token
          flyctl deploy --app practicafinaljenkins --remote-only
          '''         
        }
      }
  }
}   

     