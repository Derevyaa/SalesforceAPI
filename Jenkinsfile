pipeline {
  agent any
 
  stages {
    stage('Check installations') {
      steps {
        sh '''if which curl >/dev/null; then
    curl --version
else
sudo apt-get -y install curl
fi

        if which nodejs >/dev/null; then
    nodejs --version
else
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
#sudo apt-get -y install nodejs-legacy
 sudo apt-get -y install nodejs
fi

if which npm >/dev/null; then
    npm --version
else
    sudo apt-get -y install npm
fi

if which newman >/dev/null; then
    which newman
else
   sudo npm install -g newman
   sudo npm install -g newman-reporter-htmlextra
    # newman -version
    echo Stage 2
fi
'''
      }
    }
    stage('Test') {
      steps {
        sh '''pwd
        ls
        newman run API_Salesforce_Test.postman_collection.json -e API_Salesforce_Test.postman_environment.json -r htmlextra
'''
      }
      
    }
  }
  post {
        always {
            archiveArtifacts artifacts: 'newman/*.html', fingerprint: true
            timeout(time: 5, unit: 'MINUTES')
        }
    }
}
