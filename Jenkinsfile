pipeline {
  agent any


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

      //stage ('Build image') {
        //steps {
          //echo 'Building Docker image'
          //sh 'docker build -t practicafinaljenkins: 1.0.0 .'
        //}
      //}

      //stage ('Push Docker image to Duckerhub') {
        //steps {
          //echo 'Pushing Docker image to DockerHub'
          //sh 'echo $DH_TOKEN_PSW | docker login --username =$DH_TOKEN_USR --password-stdin'
          //sh 'docker tag practicafinaljenkins mydockerhubusername/practicafinaljenkins: 1.0.0'
          //sh 'docker push mydockehubusername/pracicafinaljenkins:1.0.0'
        //}
      //}

      stage ('Install fly.io') {
        steps {
          echo 'Installing Fly.io'
          //withCredentials([string(credentialsId: 'FLY_API_TOKEN', variable: 'FLY_TOKEN')]) {
          sh '''
            curl -L https://fly.io/install.sh | sh
            export FLYCTL_INSTALL="/var/jenkins_home/.fly"
            export PATH="$FLYCTL_INSTALL/bin:$PATH"
          '''
        }
      }

      stage ('Deploy to Fly.io') {
        steps {
          echo 'Deploying to fly.io'
          sh 'flyctl auth token $FLY_API_TOKEN'
          sh '/usr/bin/flyctl deploy --app practicafinaljenkins --remote-only'          
        }
  
      }
  }
}