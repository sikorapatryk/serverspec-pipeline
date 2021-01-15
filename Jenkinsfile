pipeline {
  agent {
    docker {
      label 'dockerhost-label'
      image 'ruby:2'
      args '-u root'
    }
  }
  stages {

    stage ('Prepare') {

      steps {
        sh '''
          gem install serverspec ed25519 bcrypt_pbkdf
          mkdir -p /root/.ssh
          chmod 500 /root/.ssh
          cp ssh_config /root/.ssh/config
        '''
      }
    }

    stage ('Test') {
      steps {
          sshagent(credentials : ['jenkins-ssh']) {
            sh '''
              rake spec
            '''
          }
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }

}
